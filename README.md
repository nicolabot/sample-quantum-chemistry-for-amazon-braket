# Quantum chemistry with Qrunch on Amazon Braket

Two hands-on notebooks that run variational quantum chemistry algorithms with
[Kvantify Qrunch](https://www.kvantify.com/products/qrunch), one of them submitting circuits
to [Amazon Braket](https://aws.amazon.com/braket/)'s managed simulator.

These are the exercise materials for the AWS workshop *Simplifying quantum chemistry with
Qrunch and Amazon Braket*. They are cloned onto participant instances at workshop start,
and they also run standalone on any machine with Qrunch installed.

## The labs

### 1. `ionization_tutorial.ipynb` — one algorithm, two machines

Compute the ionization energy of NH3 as the difference between two single-point energies:
the neutral closed-shell molecule and the open-shell doublet cation. A `(6, 4)` active
space brings that down to a circuit of about 12 qubits.

The same FAST-VQE calculation then runs two ways so you can compare them directly:

1. Local statevector simulation — free, fast, exact, the baseline.
2. Amazon Braket SV1 on-demand simulator — same linear algebra, now sampled with 1000
   shots and billed per minute of simulation time.

The point of the lab is the comparison. You measure the task count and the cost yourself
with the Braket cost tracker rather than being told what to expect.

Pointing the sampler at a real QPU is the same one-line change. That is left as an
exercise, because hardware devices retire, are billed per task plus per shot, and need a
Qrunch licence tier above the free one.

### 2. `butyronitrile_dissociation.ipynb` — one machine, four algorithms

Break the C≡N bond in butyronitrile and trace the energy across nine geometries, computing
that curve four ways: FAST-VQE, FAST-VQE with orbital optimization, BEAST-VQE with orbital
optimization, and CASCI as the exact reference. Projective embedding keeps the problem
tractable — two atoms get a wavefunction description, the rest a mean-field environment.

This lab is local simulator only. It submits nothing to Amazon Braket.

The interesting result is BEAST-VQE: half the qubits and a fraction of the runtime, landing
close to the orbital-optimized answer at the dissociated limit. Orbital optimization itself
costs roughly thirty times the standard run for a correction that only matters where the
wavefunction is multireference.

The build step caches mean-field results as `.qdk` files under `output/persister_data`, so a
second run loads instead of recomputing. Delete that directory if you change the active
space or the geometry, or you will silently reuse stale data.

## Requirements

| | |
|---|---|
| Python | 3.12 |
| Qrunch | 1.3.0 |
| Memory | 4 GB for the ionization lab. At least 16 GB for butyronitrile |
| Cores | Butyronitrile's orbital-optimization step is the only core-hungry part. Upstream recommends 12+ physical cores; fewer just means waiting longer |
| Braket | An AWS account with Amazon Braket enabled, for part 2 of the ionization lab only |

## Setup

### 1. Install Qrunch

Qrunch is proprietary software distributed from Kvantify's private package index. It is
not bundled here. Register at
[kvantify.com/products/qrunch/pricing/register](https://www.kvantify.com/products/qrunch/pricing/register)
to get access, then follow Kvantify's
[installation instructions](https://qrunch.docs.kvantify.net/docs/getting_started.html).

```bash
conda create -n qrunch-user-env python=3.12 -y
conda activate qrunch-user-env
pip install -r requirements.txt --extra-index-url <KVANTIFY_INDEX_URL>
python -m ipykernel install --user --name qrunch-user-env --display-name "Qrunch (Python 3.12)"
```

Workshop participants can skip this step. The environment is built for you during
instance provisioning.

### 2. Get your licence file

Registering with Kvantify emails you a `license.txt`. Drop it next to the notebook. The
setup cell registers it:

```python
qc.register_license_file("license.txt")
```

`license.txt` is gitignored. Do not commit it — licences are issued per person and bound
to your email address.

The free Basic tier caps the qubit count and covers this lab comfortably, since the active
space here is about 12 qubits.

### 3. Configure AWS credentials

Part 2 submits tasks to Amazon Braket and will incur charges. Braket needs a
service-linked role in the account before first use:

```bash
aws iam create-service-linked-role --aws-service-name braket.amazonaws.com
```

Braket also writes task results to an `amazon-braket-<region>-<account>` S3 bucket, which
the SDK creates on first submission if it does not exist.

## Cost

| Backend | Price | Notes |
|---|---|---|
| Local simulator | Free | Runs on your own CPU. This is all of the butyronitrile lab |
| Braket SV1 | $0.075 per minute, 3-second minimum per task | 1 hour per month is in the AWS Free Tier. Part 2 of the ionization lab only |

If you are running this on a cloud instance, the instance itself will cost you more than
Braket does. Butyronitrile's 16 GB floor is what sets that, not the quantum work.

A 12-qubit circuit finishes well inside the 3-second minimum, so the bill tracks the number
of tasks submitted rather than the difficulty of each circuit. Note that FAST-VQE's gate
selection step samples the device more than once per iteration, so the task count is higher
than `max_iterations` — the notebook prints the real figure. Confirm current pricing on the
[Amazon Braket pricing page](https://aws.amazon.com/braket/pricing/); the numbers above are
for orientation, not a quote.

## Attribution

The notebook is adapted from [Kvantify/qrunch_tutorials](https://github.com/Kvantify/qrunch_tutorials),
used under the MIT License. Kvantify wrote the chemistry. The AWS additions are the Amazon
Braket backend, the cost instrumentation, and the workshop framing.

See [THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md) for the full upstream notice.

## Licence

AWS-authored content in this repository is licensed under MIT-0. See [LICENSE](LICENSE).
Content adapted from Kvantify remains under the MIT License with its copyright notice
retained, as recorded in [THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md).
