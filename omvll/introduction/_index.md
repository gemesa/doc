+++
title = "Introduction"
weight = 10
+++

# Introduction

Welcome to the O-MVLL documentation. This documentation is split into three sections:

1. This first section, *Introduction*, is about how to use and how to get started with O-MVLL.

2. The second section, [obfuscation passes]({{< ref "/omvll/passes" >}}), describes
   the different obfuscation passes available in O-MVLL.

3. The last section, [other topics]({{< ref "/omvll/other-topics" >}}), contains different information
   for those who are already familiar with the project.

O-MVLL is an LLVM-based obfuscator that leverages the new LLVM pass manager. It integrates via 
`-fpass-plugin` in Clang or `-load-pass-plugin` option in the Swift compiler (starting with Xcode 26)
to perform native code obfuscation. The obfuscation rules are driven by a Python API defined as follows:

{{< alert type="danger" icon="fa-light fa-microchip">}}
O-MVLL currently supports **AArch64** and **AArch32** architectures.
{{</ alert >}}

```python
import omvll

class MyConfig(omvll.ObfuscationConfig):
    def __init__(self):
        super().__init__()

    def flatten_cfg(self, mod: omvll.Module, func: omvll.Function):
        if func.name == "check_password":
            return True
        return False
    def obfuscate_string(self, _, __, string: bytes):
        if string.startswith(b"/home") and string.endswith(b".cpp"):
          return "REDACTED"
```

Throughout this documentation, we use Clang to refer to the host compiler, which can be either
Clang or the Swift compiler.

{{< svg "/assets/omvll/omvll-pipeline.svg" >}}
