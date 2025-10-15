Frequently Asked Questions and Answers {#tutorial_faq}
======================================

@tableofcontents

@prev_tutorial{tutorial_linux_cmake}

**Q1: What to do if you encounter an error saying "Boost library not found" during compilation?**

A1: Please ensure that the Boost library is correctly installed and that CMake can find it. When using b2.exe to install boost, specify the installation path and add the installation path to the environment variable PATH, as shown below:

```bash
b2.exe --with-graph install --prefix=D:\boost_1_86_0
```

You need to add `D:\boost_1_86_0` to the environment variable PATH.

**Q2: What to do if you encounter an error where the `--include-docstrings` parameter is unrecognized during the compilation process?**

A2: This is due to a low version of mypy, which requires reinstallation. The following command can update mypy:

```bash
pip uninstall mypy
pip install mypy==1.14.0
```


