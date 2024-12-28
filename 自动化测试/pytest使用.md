1.命令行直接输入pytest之后，会自动执行当前目录下所有已test_开头的.py文件里的test_开头的方法

2.case执行顺序的问题：

使用pip install pytest-ordering插件，然后配置case的order值即可

比如只配置了test_pipline1这个case的order值为1时，命令行运行【pytest -v】时的运行顺序为

可以看到，只把配置了order=1的test_pipline1提出来了，而TestPipline这个py文件中另一个test_pipline2还在它应该的顺序上



```python
@pytest.mark.run(order=1)
    def test_pipline1(self):
        print('test_pipline1')
      
# 命令行运行【pytest -v】时的运行顺序为 
testcases/TestTask/test_pipline.py::TestPipline::test_pipline1 PASSED
testcases/TestAutolink/test_component.py::TestComponent::test_component1 PASSED
testcases/TestAutolink/test_component.py::TestComponent::test_component2 PASSED
testcases/TestAutolink/test_module.py::TestModule::test_module1 PASSED
testcases/TestAutolink/test_module.py::TestModule::test_module2 PASSED
testcases/TestTask/test_pipline.py::TestPipline::test_pipline2 PASSED
testcases/TestTask/test_task.py::TestTask::test_task1 PASSED
testcases/TestTask/test_task.py::TestTask::test_task2 PASSED   
```

```python
# 如果不配置test_pipline1的order的话，运行顺序为：
testcases/TestAutolink/test_component.py::TestComponent::test_component1 PASSED
testcases/TestAutolink/test_component.py::TestComponent::test_component2 PASSED
testcases/TestAutolink/test_module.py::TestModule::test_module1 PASSED
testcases/TestAutolink/test_module.py::TestModule::test_module2 PASSED
testcases/TestTask/test_pipline.py::TestPipline::test_pipline1 PASSED
testcases/TestTask/test_pipline.py::TestPipline::test_pipline2 PASSED
testcases/TestTask/test_task.py::TestTask::test_task1 PASSED
testcases/TestTask/test_task.py::TestTask::test_task2 PASSED 
```



3.scope不适合login的这种全局的最开始的case设置，如果把login写出下面的fixture的话，那在需要和这个login有前后关系的case（比如下面的test_pipline2）的参数上都要加login参数，如果case很多，几百条的话那太不优雅了

```python
@pytest.fixture(scope='session')
def login():
    pass
  
class TestPipline(object):
    def test_pipline1(self):
        print('*test_pipline1')

    def test_pipline2(self,login):
        print('*test_pipline2')
```

所以，我觉得scope适合同一个py文件中case的依赖关系设置，依赖的case数不是很多的情况

这里引申一下，@pytest.fixture(scope='session') 也可以放到conftest.py文件中，这个文件时pytest独特的文件，命名必须为conftest.py

如果确实有一些初始化什么的特殊的操作，可以用conftest.py+fixture来做到统一管理初始化的方法，但是如果知识简单的依赖，直接写在test py文件里即可，还很易读



# fixture

注意点：

1、params、参数request、返回值request.param 这都是固定写法

2、加入id后，case名回显示为id名

3、params里列表的话，testcase使用这个fixture函数的话，可以获得fixture函数的所有params执行后的返回值

```python
import pytest

@pytest.fixture(scope='module', params=['param1', 'param2', 10, 20], ids=['p1', 'p2', 'num10', 'num20'])
def test_data(request):
    """返回一个参数化的 test_data 对象，并提供自定义的 ID。"""
    request.param = str(request.param) + '111'
    return request.param

def test_example(test_data):
    """使用参数化 fixture，并展示自定义 ID。"""
    print(f"Running with test data: {test_data}")
    assert isinstance(test_data, (str, int))  # 确保 test_data 是 str 或 int 类型
```

运行结果：

```shell
test_a.py::test_example[p1] Running with test data: param1111
PASSED
test_a.py::test_example[p2] Running with test data: param2111
PASSED
test_a.py::test_example[num10] Running with test data: 10111
PASSED
test_a.py::test_example[num20] Running with test data: 20111
PASSED
```



4、fixture有scope：session、muddle、class、function，设定的范围内所有case运行前执fixture函数

5、fixture scope设置为class，这个testclass里每个case都写了fixture函数名，则在class前执行一次fixture，也只执行一次哦，如果testclass的第2个case写了fixture函数名，则这个fixture在第二个testcase执行之前执行，是在第一个case后哦

6、fixture可以多层嵌套，但是使用fixture的testcase的返回值，只看当前函数使用的fixture的返回值





