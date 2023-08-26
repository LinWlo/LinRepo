# 测试相关

## 1. pytest使用

### 1.1. 基本使用

- **第三方模块**：pytest，需要安装。
- **使用说明**
  - 文件名尽量以test_开头或结尾;
  - 函数名必须以test_开头;
  - 类名必须以Test开头，方法名必须以test_开头，类中不能重写构造方法;
  - 使用assert进行断言，语法 `assert 布尔表达式`;
    - 布尔表达式为真则通过，为假报错，pytest就能根据断言结果，从而统计测试用例失败还是通过。
- **使用步骤**
  - 导入模块：`import pytest`
  - 编写测试用例（函数或者方法），每一个函数或者方法就是一个测试用例。
  - 启动测试：`pytest.main(['参数1','参数2',……,'file/dir'])`
    - `file/dir`，如果指定的是file，则自动加载该文件下所有满足命名规范的函数或者方法；如果指定的是目录dir，则自动加载该目录下所有test开头的文件中的满足命名规范的函数或者方法。
    - 输出运行日志
      - `-q`：显示简略信息（只显示通过数和失败数（失败的信息会显示出来））。
      - `-s`：显示测试用例中print语句的输出结果。
      - `-v`：显示详细信息（不包括-s的显示信息）。
  - 命令行（cmd）方式启动：
    - `pytest 参数 脚本`
    - `python -m pytest 参数 脚本`
- **常用参数**
  - `-k`：指定关键字，该关键字可以匹配文件名、函数名、类名、方法名，执行匹配到的关键字。
    - `-k 关键字`
  - `-x`：在执行测试过程中一旦出现fail的用例，则立即停止测试执行。
  - `--maxfail=n`：在执行测试过程中一旦出现fail的用例达到n条，则立即停止测试执行。
  - `-n`：如`-n=5`，支持多线程的分布式运行测试用例（用例多时可以使用）。
  - `-m`：`-m 标签名`  指定用例标记，指定后只会执行带有该标记的用例。
    - `-m 标记名`
    - 如冒烟测试用例，系统测试用例，标记好后，做冒烟测试，就只执行冒烟测试的用例
    - 如何标记用例？
      - 在项目根目录下创建文件 `pytest.ini`，在该文件中添加用例标记。多个标记名需要换行且有缩进。

      ```python
      [pytest]
      markers = smoke
      abc
      efg
      ```

    - 通过装饰器 `@pytest.mark.标记名` 来标记用例（可以作用于函数、类、方法）。
- **跳过用例**
  - `@pytest.mark.skip(reason='msg')`：无条件的跳过用例。
  - `@pytest.mark.skipif(布尔表达式,reason='msg')`：当布尔表达式结果为True，跳过用例。
  - `pytest.skip('msg')`：在测试用例内部根据实际使用情况跳过用例。
- **失败用例重跑**
  - 第三方模块：`pytest-rerunfailures`，需要安装，不用导入，pytest框架会自动导入。
  - 通过装饰器 `@pytest.mark.flaky(reruns=n,reruns_delay=t)`实现用例失败后的重跑。
    - `reruns=n`：指定重跑的次数
    - `reruns_delay=t`：指定每次运行的间隔时间，单位s
- **参数化**
  - 通过装饰器 @pytest.mark.parametrize('用例形参1,…',values) 实现参数操作。
    - `'用例形参1,…'`：必须与被装饰的函数或者方法的形参保持一致。
      - 注意多个形参在一对引号内。
      - `values`：测试数据，通常以列表传入。
      - 会将列表中的每个元素传给形参，多少个元素就跑多少次用例。
- **前置与后置**
  - 模块级
    - `setup_moudle()`：模块级前置，在当前模块中所有测试用例执行前运行1次。
    - `teardown_moudle()`：模块级后置，在当前模块中所有测试用例执行后运行1次。
  - 函数级
    - `setup_function()`：函数级前置，在当前模块中每一条测试用例执行前运行1次。
    - `teardown_function()`：函数级后置，在当前模块中每一条测试用例执行后运行1次。
  - 类级
    - `setup_class()`：类级前置，在当前类中所有测试用例执行前运行1次。
    - `teardown_class()`：类级后置，在当前类中所有测试用例执行后运行1次。
  - 方法级
    - `setup()/setup_method()`：方法级前置，在当前类中每一条测试用例执行前运行1次。
    - `teardown/teardown_method()`：方法级后置，在当前类中每一条测试用例执行后运行1次
- **fixture**
  - 基本使用
    - 在项目下创建文件 `conftest.py`，在该文件中编写fixture
    - 通过装饰器 `@pytest.fixture(name,scope,params,autouse)` 实现。
      - `name`：指定fixture的名称，如果不指定则默认为被装饰的函数名。
      - `scope`：指定fixture的作用范围，包括package、session、moudle、class、function（默认）。
      - `params`：用于接收外部传入的参数。
      - `autouse`：自动调用。
    - 调用：
      - 在测试函数或者方法中通过fixture的名字进行调用。
      - 在fixture函数中使用 autouse 默认是False，使用True表示启用，对测试脚本生效。
      - 直接在用例头上使用 @pytest.mark.usefixtures("这里传入你自己定义好的fixture函数名")。
        - 注意：传入的是字符串
    - 例子：通过fixture实现前后置，在fixture中实现不同用户的登陆前置（为fixture传参）
      - `conftest.py`中在被装饰的函数中添加形参 request，在函数内部通过`request.param`获取到外部传入的数据。
      - 在测试函数或者方法中调用fixture时使用 `pytest.mark.parametrize('fixture名称',[[value1,….]],indirect=True)` 传参。
      `-`indirect=True`：告诉系统 fixture名称是一个fixture，不能把它作为一个普通参数处理。

      ```python
      # conftest.py
      import pytest,requests
      @pytest.fixture(name='lg',scope='function')
      def login(request):
          rq = requests.session()
          #前置，实现登陆操作
          data = request.param  #获取外部传入的数据
          print(data) #根据传入的数据类型来处理数据
          url = 'http://localhost:80/login'
          info = {'username':data[0],'password':data[1],'verifycode':'0000'}
          rq.post(url=url, data=info)
          #通过yield返回session对象
          yield rq
          #后置，实现注销操作
          url = 'http://localhost:80/logout'
          rq.get(url=url)
      
      # test_ab.py 给fixture传参实现不同账号登陆
      @pytest.mark.parametrize('lg',[['admin','admin123']],indirect=True)     #给fixture传参
      @pytest.mark.parametrize('a',[1,2,3,4,5])
      def test_caseA(lg,a):
          assert 1==1

      """test_caseA和test_caseB两条不同的测试用例实现了使用不同的账号登陆，通过为fixture传参"""

      @pytest.mark.parametrize('lg',[['dy','123123']],indirect=True)     #给fixture传参
      @pytest.mark.parametrize('b',[1,2,3,4,5])
      def test_caseB(lg,b):
          assert 1==1
      ```

- **测试报告**
  - 模块：`pip install pytest-html`
  - 使用：在 `pytest.main([])`中添加参数 `--html=path/report_name.html`
    - `path`：报告保存的路径
    - `report_name.html`：报告名称

## 2. python实现ssh登陆

```python

import paramiko
import pytest

# 登录接口的基本信息
SSH_HOST = '192.168.42.129'
SSH_PORT = 22
SSH_USERNAME = 'root'
SSH_PASSWORD = 'root'

def test_ssh_login_success():
    # 创建SSH客户端
    client = paramiko.SSHClient()
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

    try:
        client.connect(hostname=SSH_HOST, port=SSH_PORT, username=SSH_USERNAME, password=SSH_PASSWORD)
        #  exec_command 方法来执行远程命令。该方法返回一个三元组，包含了命令的标准输入、标准输出和标准错误输出。
        stdin, stdout, stderr = client.exec_command('tail -3 /var/log/audit/audit.log')
        output = stdout.read().decode().strip()
    except paramiko.AuthenticationException:
        print("ssh连接失败")
        output = ''

    finally:
        client.close()
    assert 'hostname=192.168.42.1 addr=192.168.42.1 terminal=ssh res=success' in output

if __name__ == '__main__':
    pytest.main(['-s'])
```

## 3. 并发执行命令

- 有时候，我们需要同时执行多个命令，并等待它们的结果。paramiko 提供了 Channel 类来实现并发执行命令

```python
import paramiko

# 创建 SSH 客户端对象
client = paramiko.SSHClient()

# 允许连接到不在 known_hosts 文件中的主机
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

# 连接到远程主机
client.connect('localhost', username='your_username', password='your_password')

# 打开一个 SSH 会话通道
channel = client.get_transport().open_session()

# 并发执行多个命令
commands = ['ls', 'pwd', 'whoami']
for command in commands:
    # 执行命令并等待结果
    channel.exec_command(command)
    exit_status = channel.recv_exit_status()

    # 读取命令的输出
    output = channel.recv(4096).decode('utf-8')

    # 打印输出结果
    print(f'Command: {command}')
    print(f'Exit status: {exit_status}')
    print(f'Output:\n{output}')

# 关闭连接
client.close()
```
