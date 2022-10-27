# LLVM Tablegen Grammer

## content

tblgen 由``*.td``文件生成``*.inc``文件。

```bash
cd lib/Target/AMDGPU
../../../build/bin/llvm-tblgen AMDGPU.td -I=/home/loongson/work/gpu/llvm-toolchain-8-8.0.1/llvm/include
```

### Value

``类型 + 值名字``

```
string Greeting = "hello";
```

### let

``let + 值名字 = 新的值内容``

```
class D : C { let V = 0; }
def Z : D;
```

> 这个例子中，父类C中包含的V值在子类D中被重新赋值为0，而Z是D的实现，从而Z中的V值的内容为0。

## class and def

```
class C { bit V = 1; }
def X : C;
def Y : C {
 string Greeting = "hello";
}
```

> 这个例子中定义了两条记录：X和Y，这两条记录具有相同的信息V，所以用了一个类C来实现共有部分，同时，还扩展了记录Y，使它多了一个Greeting的字符串。

### class template

```
class FPFormat<bits<3> val> {
 bits<3> Value = val;
}
def NotFP   : FPFormat<0>;
def ZeroFP   : FPFormat<1>;
def OneArgFP    : FPFormat<2>;
def OneArgFPRW  : FPFormat<3>;
def TwoArgFP    : FPFormat<4>;
def CompareFP   : FPFormat<5>;
def CondMovFP   : FPFormat<6>;
def SpecialFP   : FPFormat<7>;
```

----

**example**:

```
class ModRefVal<bits<2> val> {
  bits<2> Value = val;
}

def None   : ModRefVal<0>;
def Mod    : ModRefVal<1>;
def Ref    : ModRefVal<2>;
def ModRef : ModRefVal<3>;

class Value<ModRefVal MR> {
  // Decode some information into a more convenient format, while providing
  // a nice interface to the user of the "Value" class.
  bit isMod = MR.Value{0};
  bit isRef = MR.Value{1};

  // other stuff...
}

// Example uses
def bork : Value<Mod>;
def zork : Value<Ref>;
def hork : Value<ModRef>;
```

**test on terminal**:

```bash
llvm-tblgen --print-records example.td
```

**output**:

```
def bork {      // Value
  bit isMod = 1;
  bit isRef = 0;
}
def hork {      // Value
  bit isMod = 1;
  bit isRef = 1;
}
def zork {      // Value
  bit isMod = 0;
  bit isRef = 1;
}
```

### multiclass

multiclass并不是指多个类，而是指用一个类结构来实现多个类的功能。

```
def ops;
def GPR;
def Imm;
class inst<int opc, string asmstr, dag operandlist>;

multiclass ri_inst<int opc, string asmstr> {
  def _rr : inst<opc, !strconcat(asmstr, " $dst, $src1, $src2"),
                 (ops GPR:$dst, GPR:$src1, GPR:$src2)>;
  def _ri : inst<opc, !strconcat(asmstr, " $dst, $src1, $src2"),
                 (ops GPR:$dst, GPR:$src1, Imm:$src2)>;
}

// Instantiations of the ri_inst multiclass.
defm ADD : ri_inst<0b111, "add">;
defm SUB : ri_inst<0b101, "sub">;
defm MUL : ri_inst<0b100, "mul">;
```

> 我们就能用一个multiclass来实现3地址指令模版的共有部分，然后用一个defm来定义这两种不同的指令模式。

> 最终的记录的名字是由defm后边的名字和``multiclass``中``def关键字后边的名字``拼接而成的，也就是实际上定义了``ADD_rr``、``ADD_ri``、``SUB_rr``、``SUB_ri``、``MUL_rr``、``MUL_ri``等指令。

**example2**:

```
class Instruction<bits<4> opc, string Name> {
  bits<4> opcode = opc;
  string name = Name;
}

multiclass basic_r<bits<4> opc> {
  def rm : Instruction<opc, "rm">;
  def rr : Instruction<opc, "rr">;
}

multiclass basic_s<bits<4> opc> {
  defm SD : basic_r<opc>;
  defm SS : basic_r<opc>;
  def X : Instruction<opc, "x">;
}

multiclass basic_p<bits<4> opc> {
  defm PD : basic_r<opc>;
  defm PS : basic_r<opc>;
  def Y : Instruction<opc, "y">;
}

defm ADD :  basic_p<0xf>, basic_s<0xf>;
```

> 实际上定义的是：ADDPDrm、ADDPDrr、ADDPSrm、ADDPSrr、ADDSDrm、ADDSDrr、ADDSSrm、ADDSSrr、ADDY、ADDX这几个指令。

**example3**：

```
class XD { bits<4> Prefix = 11; }
class XS { bits<4> Prefix = 12; }

class I<bits<4> op> {
  bits<4> opcode = op;
}

multiclass R {
  def rm : I<2>;
  def rr : I<4>;
}

multiclass Y {
  defm SD : R, XS;
  defm SS : R, XD;
}

defm Instr : Y;
```

**output3**:

```
def InstrSDrm {
  bits<4> opcode = { 0, 0, 1, 0 };
  bits<4> Prefix = { 1, 1, 0, 0 };
}
def InstrSDrr {
  bits<4> opcode = { 0, 1, 0, 0 };
  bits<4> Prefix = { 1, 1, 0, 0 };
}
def InstrSSrm {
  bits<4> opcode = { 0, 0, 1, 0 };
  bits<4> Prefix = { 1, 0, 1, 1 };
}
def InstrSSrr {
  bits<4> opcode = { 0, 1, 0, 0 };
  bits<4> Prefix = { 1, 0, 1, 1 };
}
```

#### let

``let ... in { ... }``表示把多个记录包含在大括号内。

```
let isTerminator = 1, isReturn = 1, isBarrier = 1, hasCtrlDep = 1 in
  def RET : I<0xC3, RawFrm, (outs), (ins), "ret", [(X86retflag 0)]>;

let isCall = 1 in
  // All calls clobber the non-callee saved registers...
  let Defs = [EAX, ECX, EDX, FP0, FP1, FP2, FP3, FP4, FP5, FP6, ST0,
              MM0, MM1, MM2, MM3, MM4, MM5, MM6, MM7,
              XMM0, XMM1, XMM2, XMM3, XMM4, XMM5, XMM6, XMM7, EFLAGS] in {
    def CALLpcrel32 : Ii32<0xE8, RawFrm, (outs), (ins i32imm:$dst,variable_ops), "call\t${dst:call}", []>;
    def CALL32r     : I<0xFF, MRM2r, (outs), (ins GR32:$dst, variable_ops), "call\t{*}$dst", [(X86call GR32:$dst)]>;
    def CALL32m     : I<0xFF, MRM2m, (outs), (ins i32mem:$dst, variable_ops), "call\t{*}$dst", []>;
}
```

> Let表达式在文件内经常用于在一系列的记录中增加一些定义，这些记录不需要被展开，就像上例中，那几个CALL指令，没有展开即加入了``isCall=1``和``Defs=[…]``的定义属性。

#### foreach

```
foreach i = [0, 1, 2, 3] in {
  def R#i : Register<...>;
  def F#i : Register<...>;
}
```

> 经过4次循环，分别定义了：R0、R1、R2、R3、F0、F1、F2、F3
