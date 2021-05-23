To upload to Pypi:

```shell
docker run --rm -v (pwd):/io konstin2/maturin publish -r https://test.pypi.org/legacy/ -u USER -p PASSWORD
```

To upload to Test Pypi:

```shell
docker run --rm -v (pwd):/io konstin2/maturin publish -u USER -p PASSWORD
```
