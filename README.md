# Parse Semver

Github action that can be used to parse a semver string. It leverages [semver](https://www.npmjs.com/package/semver) behind the scene.

## Versioning in repository

> Version in this repository means annotated git tag named according to these guidelines.

Currently, versioning will be done automatically on master. Pull requests against master are checked to contain one of the following labels:

- `bump:patch`
- `bump:minor`
- `bump:major`

In repository, following versioning needs to be followed:

### Normal semantic versioning after every merge

With [semver](http://semver.org) style, every pull request merge will bump version number in following style:

- *patch*, bugfix, typo, configuration change in non-production: micro version
- *minor*, backward-compatible new feature, backward-compatible feature enhancement, configuration change: minor version
- *major*, non-backward compatible change, removal of a feature: major version

Versioning will follow naming of *vMAJOR.MINOR.PATCH*

## Inputs

### `input_string`

**Required** Raw input string to be parsed. Its a string either containing a semver or is a semver itself.

### `version_extractor_regex`

**Optional** Regex pattern to extract semver substring from the raw input. It is mandatory to match a single substring from the input string. 

## Outputs

### `major`

`MAJOR` part of the semver. Eg. `3` in  `3.4.5-alpha+1.2`


### `minor`

`MINOR` part of the semver. Eg. `4` in  `3.4.5-alpha+1.2`


### `patch`

`PATCH` part of the semver. Eg. `5` in  `3.4.5-alpha+1.2`


### `prerelease`

`prerelease` part of the semver. Eg. `alpha` in  `3.4.5-alpha+1.2`


### `build`

`build` part of the semver. Eg. `1.2` in  `3.4.5-alpha+1.2`


### `fullversion`

Full representation of the semver. For eg. `3.4.5-alpha+1.2` in  `3.4.5-alpha+1.2`

## Example usage

### With an extractor regex 
```
...
job:
  test_semver_parser:
    runs_on: ubuntu_latest
    steps:
     - name: Parse semver string
       id: semver_parser 
       uses: booxmedialtd/ws-action-parse-semver@v1
       with:
         input_string: 'refs/tags/v1.0.4'
         version_extractor_regex: '\/v(.*)$'
     - name: Use parsed semver
       run: |
            echo "v${{ steps.semver_parser.outputs.major }}"
            echo "v${{ steps.semver_parser.outputs.minor }}"
            echo "v${{ steps.semver_parser.outputs.patch }}"
            echo "v${{ steps.semver_parser.outputs.prerelease }}"
            echo "v${{ steps.semver_parser.outputs.build }}"
            echo "v${{ steps.semver_parser.outputs.fullversion }}"
```

### Without an extractor regex. Note: `input_string` must be a valid semver string.
```
...
job:
  test_semver_parser:
    runs_on: ubuntu_latest
    steps:
     - name: Parse semver string
       id: semver_parser 
       uses: booxmedialtd/ws-action-parse-semver@v1
       with:
         input_string: '1.0.4'
     - name: Use parsed semver
       run: |
            echo "v${{ steps.semver_parser.outputs.major }}"
            echo "v${{ steps.semver_parser.outputs.minor }}"
            echo "v${{ steps.semver_parser.outputs.patch }}"
            echo "v${{ steps.semver_parser.outputs.prerelease }}"
            echo "v${{ steps.semver_parser.outputs.build }}"
            echo "v${{ steps.semver_parser.outputs.fullversion }}"
```

## License
This library is under the MIT license.
