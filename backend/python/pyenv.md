# Pyenv

{% hint style="warning" %}
Pyenv 在 MacOS 上安装 Python 版本有些许不一样
{% endhint %}

```
PYTHON_CONFIGURE_OPTS="--enable-framework" pyenv install 3.6.0
```

如果不适用上面的 `PYTHON_CONFIGURE_OPTS="--enable-framework"` 会在后序流程中出现如下错误

```
OSError: Python library not found: Python, .Python, libpython3.6m.dylib, libpython3.6.dylib
This would mean your Python installation doesn't come with proper library files.
This usually happens by missing development package, or unsuitable build parameters of Python installation.

* On Debian/Ubuntu, you would need to install Python development packages
  * apt-get install python3-dev
  * apt-get install python-dev
* If you're building Python by yourself, please rebuild your Python with `--enable-shared` (or, `--enable-framework` on Darwin)
```
