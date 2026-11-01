# No Matrix Support Reproduction

## Expected

Renovate would detect both containers being used here:

```yaml
  does-not-update:
    strategy:
      matrix:
        include:
          - container: quay.io/pypa/manylinux_2_28_aarch64:2026.10.31-1@sha256:724b504247662e1cf122590957c857211ec2d16b113ec134f92c570a2d01250f

    runs-on: ubuntu-latest
    container: ${{ matrix.container }}

...

  does-update:
    runs-on: ubuntu-latest
    container:
      image: quay.io/pypa/manylinux_2_28_aarch64:2026.10.31-1@sha256:724b504247662e1cf122590957c857211ec2d16b113ec134f92c570a2d01250f
```

And try to update both.

## Actual

Renovate only detects and updates the container in the `does-update` job.

## Additional Context

`zizmor` just recently gained some improved support for detecting indirect
values from matrices, and can flag `unpinned-images` in the example below:

```yaml
  unpinned:
    strategy:
      matrix:
        include:
          - container: quay.io/pypa/manylinux_2_28_aarch64

    runs-on: ubuntu-latest
    container: ${{ matrix.container }}
```

See: <https://github.com/zizmorcore/zizmor/pull/2395>

Pinning images is a good idea, but it's difficult to justify if the only way to
update those pins is manually.

`rust-lang/rust-analyzer` currently uses several containers through a matrix
like this, and I'd like to propose pinning those containers. It wouldn't really
make sense to propose if there isn't a good way to keep those pins updated,
though.

See: <https://github.com/rust-lang/rust-analyzer/blob/b98172d80f0da12bbde3d6733c274e17cb6417aa/.github/workflows/release.yaml#L24-L73>
