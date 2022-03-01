# 插件
```python
def pytest_addoption(parser):
    parser.addoption("--step", type=str, default="")
    parser.addoption("--taskid", type=str, default="")


def pytest_generate_tests(metafunc):
    # This is called for every test. Only get/set command line arguments
    # if the argument is specified in the list of test "fixturenames".
    step_value = metafunc.config.option.step
    taskid_value = metafunc.config.option.taskid
    if "step" in metafunc.fixturenames and step_value is not None:
        metafunc.parametrize("step", [step_value])
    if "taskid" in metafunc.fixturenames and taskid_value is not None:
        metafunc.parametrize("taskid", [taskid_value])


def pytest_collection_modifyitems(items):
    """
    测试用例收集完成时，将收集到的item的name和nodeid的中文显示在控制台上
    :return:
    """
    for item in items:
        # 只需要对中括号内的数据做一个编码转换即可，防止因为测试用例本身就是中文导致编码再出问题
        names = item.name.split("[")
        # nodeids = item.nodeid.split("[")
        item.name = names[0]
```