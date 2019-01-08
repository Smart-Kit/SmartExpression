# SmartExpression

## 运算符

| 运算符 | 描述 |
| ------------- |:-------------:|
| + | Add |
| & | And |
| && | AndAlso |
| / | Divide |
| == | Equal |
| ^ | ExclusiveOr |
| > | GreaterThan |
| >= | GreaterThanOrEqual |
| << | LeftShift |
| ^ | ExclusiveOr |
| < | LessThan |
| <= | LessThanOrEqual |
| . | MemberAccess |
| % | Modulo |
| * | Multiply |
| ! | Not |
| != | NotEqual |
| | | Or |
| &#124;&#124; | OrElse |
| ^ | Power |
| >> | RightShift |
| - | Subtract |
| ?? | Coalesce a??b |
| [i] | ArrayIndex |

## 关键词

| 关键词 | 描述 |
| --- |:------:|
| as | TypeAs |
| is | TypeIs |

## 类型关键词

> null,sbyte,byte,char,short,ushort,int,uint,long,ulong,float,double,decimal,bool,true,false

## SmartExpression 表达式语法

``` csharp
public class CompileFactoryTest
    {
        [Fact]
        public void Compile_Bool()
        {
            var exe = CompileFactory.Compile<Func<object, bool>>("Id==1");
            bool result = exe.Invoke(new { Id = 1 });
            Assert.True(result);
        }

        [Fact]
        public void Compile_Bool_WithNoParamters()
        {
            var exe = CompileFactory.Compile<Func<bool>>("1==1");
            bool result = exe.Invoke();
            Assert.True(result);
        }

        [Fact]
        public void Compile_Bool_IsNotNull()
        {
            var exe = CompileFactory.Compile<Func<object, bool>>("Name!=null");
            bool result = exe.Invoke(new
            {
                Name = "SmartExpression"
            });
            Assert.True(result);
        }

        [Fact]
        public void Compile_Bool_NestIsNotNull()
        {
            var exe = CompileFactory.Compile<Func<object, bool>>("Who.Name!=null");
            bool result = exe.Invoke(new
            {
                Who = new
                {
                    Name = "SmartExpression"
                }
            });
            Assert.True(result);
        }

        [Fact]
        public void Compile_Bool_Array()
        {
            var exe = CompileFactory.Compile<Func<object, bool>>("Ids.Length==3 and Ids[0]==1");
            bool result = exe.Invoke(new
            {
                Ids = new long[] { 1, 2, 3 }
            });
            Assert.True(result);
        }

        [Fact]
        public void Compile_Bool_NestBlock()
        {
            var exe = CompileFactory.Compile<Func<object, bool>>("Ids!=null && (Ids.Length==3 && Ids[0]==1) && Name!=null");
            bool result = exe.Invoke(new
            {
                Ids = new long[] { 1, 2, 3 },
                Name = "SmartExpression"
            });
            Assert.True(result);
        }

        [Fact]
        public void Compile_Int()
        {
            var exe = CompileFactory.Compile<Func<int>>("1+1");
            int result = exe.Invoke();
            Assert.Equal(2, result);
        }
```
