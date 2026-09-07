# Third-party licences

## Kvantify qrunch_tutorials

The following files are adapted from
[Kvantify/qrunch_tutorials](https://github.com/Kvantify/qrunch_tutorials) and are used
under the MIT License:

| File in this repository | Upstream source |
|---|---|
| `ionization_tutorial.ipynb` | `ionization-tutorial/ionization_tutorial.ipynb` |
| `data/nh3_eq.xyz` | `ionization-tutorial/data/nh3_eq.xyz` (geometry replaced with the experimental equilibrium structure) |
| `butyronitrile_dissociation.ipynb` | `butyronitrile-dissociation/butyronitrile_dissociation.ipynb` |
| `data/butyronitrile_dissociation.xyz` | `butyronitrile-dissociation/data/butyronitrile_dissociation.xyz` (unmodified) |
| `data/butyronitrile_dissociation_large.xyz` | `butyronitrile-dissociation/data/butyronitrile_dissociation_large.xyz` (unmodified) |

Both notebooks are derivative works, not copies. The chemistry, the problem construction
and the algorithm configuration are Kvantify's.

In `ionization_tutorial.ipynb` the AWS additions are the Amazon Braket backend, the cost
instrumentation with `braket.tracking.Tracker`, and the workshop framing and commentary.
Upstream's QPU section is not carried here.

In `butyronitrile_dissociation.ipynb` the AWS additions are the workshop framing, the
hardware guidance and the commentary. The calculations are unchanged from upstream and it
submits nothing to Amazon Braket. The two geometry files are carried verbatim.

```
MIT License

Copyright (c) 2025 Kvantify

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Qrunch SDK

Qrunch is proprietary commercial software from [Kvantify](https://www.kvantify.com/). It
is not distributed with this repository. Participants install it from Kvantify's package
index under the licence terms they accept when registering with Kvantify.

## Amazon Braket SDK

The [Amazon Braket Python SDK](https://github.com/amazon-braket/amazon-braket-sdk-python)
is licensed under the Apache License 2.0. It is installed from PyPI, not vendored here.
