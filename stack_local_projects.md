In __project a__ run the command
```
> stack sdist
```
that will package your project up into a tarball and show you what directory it is in.

Then go to __project b__ and add the following to the `stack.yaml` file under the `extra-deps` section

```yaml
extra-deps:
- archive: /path/to/project-a/tar/file
```
and in your `package.yaml` file under dependencies
```yaml
dependencies:
# other dependencies
- project-a == 1.0.0
```

then run
```
> stack build
```
and project-a will be included in project-b.