1. ## class argparse.ArgumentParser 的参数

- **<font color='green'>prog</font>**- 程序名称（默认：sys.argv[0]）
- **<font color='green'>usage</font>** - 描述程序使用情况的字符串（默认值：从添加到解析器的参数生成）
- **<font color='green'>description</font>** - 在参数帮助前显示的文本（默认值：无）
- **<font color='green'>epilog</font>** - 在参数help之后显示的文本（默认值：无）
- **<font color='green'>parent</font>** - 还应包含其参数的对象列表 [ArgumentParser]
- **<font color='green'>formatter_class</font>** - 用于自定义帮助输出的类
- **<font color='green'>prefix_chars</font>** - 前缀可选参数的字符集（默认"-")
- **<font color='green'>argument_default</font>** - 为参数的全局默认值（默认None）
- **<font color='green'>conflict_handler</font>** - 解决冲突的可选内容的策略（通常是不必要的）
- **<font color='green'>add_help</font>** - 添加`-h/--help`选项解析器（默认值：`True`）
- **<font color='green'>allow_abbrev</font>** - 如果缩写是明确的，则允许缩写长选项。（默认值：`True`）

```python
import argpatse

# 1.实例化类ArgumentParser,description形容这个命令的作用
parser = argparse.ArgumentParser(description='my functions')

# 2.实例化类ArgumentParser, 参数prog
parser = argparse.ArgumentParser(prog='myprogram')
parser.print_help()
# 输出：usage: myprogram [-h]

# 3.实例化类ArgumentParser, 参数prog，usage
parser = argparse.ArgumentParser(prog='PROG', usage='%(prog)s [options]')

```

2. ## 方法： add_argument参数

- **<font color='green'>name or flags</font>** - 一组name或可选字符串列表，如 foo 或者 -f, --foo
- **<font color='green'>action</font>** - 在命令行中遇到此参数要采取的基本类型的动作
- **<font color='green'>nargs</font>** - 应该使用的命令行参数的数量
- **<font color='green'>const</font>** - 有些需要一个恒定值，action 和 nargs 选择
- **<font color='green'>default</font>** - 如果参数是通过命令行不存在产生的值
- **<font color='green'>type</font>** - 命令行参数应转换为的类型
- **<font color='green'>choices</font>** - 一个参数允许值的容器
- **<font color='green'>required</font>** - 是否可以省略命令行选项（仅可选）
- **<font color='green'>help</font>** - 做什么的简短说明
- **<font color='green'>metavar</font>** - 使用情况消息中参数的名称
- **<font color='green'>dest</font>** - 要添加到由 <font color='pink'> parse_args() </font>返回的对象的属性的名称

##### 1）name or flags

```python
parser = argparse.ArgumentParser(prog='PROG')
# name or flags
parser.add_argument('-f', '--foo')
parser.add_argument('bar')
parser.parse_args(['BAR'])
# ^输出：Namespace(bar='BAR', foo=None)
parser.parse_args(['BAR', '--foo', 'FOO'])
# ^输出：Namespace(bar='BAR', foo='FOO')
parser.parse_args(['--foo', 'FOO'])
# ^输出：usage: PROG [-h] [-f FOO] bar
#	    PROG: error: the following arguments are required: bar
```

##### 2）action

```python
# action：'store'-仅仅保存这个参数值，是默认选择
parser = argparse.ArgumentParser()
parser.add_argument('--foo')
parser.parse_args('--foo 1'.split())
# ^输出：Namespace(foo='1')

# action: 'store_const' - 将存储由const关键字参数指定的值
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', action='store_const', const=42)
>>> parser.parse_args(['--foo'])
Namespace(foo=42)

# action：'store_true'和'store_false'分别用于存储值 true 和 false
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', action='store_true')
>>> parser.add_argument('--bar', action='store_false')
>>> parser.add_argument('--baz', action='store_false')
>>> parser.parse_args('--foo --bar'.split())
Namespace(foo=True, bar=False, baz=True)

# action：'append' - 将存储列表，将每个参数值都附加到列表中，允许多次选定选项
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', action='append')
>>> parser.parse_args('--foo 1 --foo 2'.split())
Namespace(foo=['1', '2'])

# action：'append_const' - 存储一个列表，并将 const 关键字指定的值附加到列表中。 const 默认为None
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--str', dest='types', action='append_const', const=str)
>>> parser.add_argument('--int', dest='types', action='append_const', const=int)
>>> parser.parse_args('--str --int'.split())
Namespace(types=[<class 'str'>, <class 'int'>])

# action：'count' - 计算关键字参数出现的次数
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--verbose', '-v', action='count', default=0)
>>> parser.parse_args(['-vvv'])
Namespace(verbose=3)

# action: 自定义
>>> class FooAction(argparse.Action):
...     def __init__(self, option_strings, dest, nargs=None, **kwargs):
...         if nargs is not None:
...             raise ValueError("nargs not allowed")
...         super(FooAction, self).__init__(option_strings, dest, **kwargs)
...     def __call__(self, parser, namespace, values, option_string=None):
...         print('%r %r %r' % (namespace, values, option_string))
...         setattr(namespace, self.dest, values)
```

##### 3）nargs

```python
# nargs: N ,一个整数，命令行中的参数将收集到一个列表中
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', nargs=2)
>>> parser.add_argument('bar', nargs=1)
>>> parser.parse_args('c --foo a b'.split())
Namespace(bar=['c'], foo=['a', 'b'])

# nargs： '?'，如果可能，将从命令行使用一个参数，并将其作为单个项目产生。如果不存在命令行参数，则将生成默认值。
    # 例1
    >>> parser = argparse.ArgumentParser()
    >>> parser.add_argument('--foo', nargs='?', const='c', default='d')
    >>> parser.add_argument('bar', nargs='?', default='d')
    >>> parser.parse_args(['XX', '--foo', 'YY'])
    Namespace(bar='XX', foo='YY')
    >>> parser.parse_args(['XX', '--foo'])
    Namespace(bar='XX', foo='c')
    >>> parser.parse_args([])
    Namespace(bar='d', foo='d')
    # 例2
    >>> parser = argparse.ArgumentParser()
    >>> parser.add_argument('infile', nargs='?', type=argparse.FileType('r'),
    ...                     default=sys.stdin)
    >>> parser.add_argument('outfile', nargs='?', type=argparse.FileType('w'),
    ...                     default=sys.stdout)
    >>> parser.parse_args(['input.txt', 'output.txt'])
    Namespace(infile=<_io.TextIOWrapper name='input.txt' encoding='UTF-8'>,
              outfile=<_io.TextIOWrapper name='output.txt' encoding='UTF-8'>)
    >>> parser.parse_args([])
    Namespace(infile=<_io.TextIOWrapper name='<stdin>' encoding='UTF-8'>,
              outfile=<_io.TextIOWrapper name='<stdout>' encoding='UTF-8'>)

# nargs: '*', 存在的所有命令行参数都收集到一个列表中。请注意，使用多个位置参数通常没有多大意义nargs='*'，但可以使用多个可选参数nargs='*'
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('--foo', nargs='*')
>>> parser.add_argument('--bar', nargs='*')
>>> parser.add_argument('baz', nargs='*')
>>> parser.parse_args('a b --foo x y --bar 1 2'.split())
Namespace(bar=['1', '2'], baz=['a', 'b'], foo=['x', 'y'])

# nargs: '+',就像一样'*'，所有存在的命令行参数都被收集到一个列表中。此外，如果没有至少一个命令行参数，则会生成一条错误消息。
>>> parser = argparse.ArgumentParser(prog='PROG')
>>> parser.add_argument('foo', nargs='+')
>>> parser.parse_args(['a', 'b'])
Namespace(foo=['a', 'b'])
>>> parser.parse_args([])
usage: PROG [-h] foo [foo ...]
PROG: error: the following arguments are required: foo
            
# nargs: argparse.REMAINDER。所有其余的命令行参数都收集到一个列表中。这对于分派到其他命令行实用程序的命令行实用程序通常很有用
>>> parser = argparse.ArgumentParser(prog='PROG')
>>> parser.add_argument('--foo')
>>> parser.add_argument('command')
>>> parser.add_argument('args', nargs=argparse.REMAINDER)
>>> print(parser.parse_args('--foo B cmd --arg1 XX ZZ'.split()))
Namespace(args=['--arg1', 'XX', 'ZZ'], command='cmd', foo='B')
```



