<p align='right'>
<a href='https://drone.io/speter-go1/gcfg/latest'><img width='80' align='absmiddle' height='18' src='https://drone.io/speter-go1/gcfg/status.png' /></a>
<a href='http://godoc.org/code.google.com/p/gcfg'><img width='94' align='absmiddle' height='18' alt='GoDoc' src='https://godoc.org/code.google.com/p/gcfg?status.png' /></a>
</p>


## What is gcfg? ##

Gcfg is a Go library that reads text-based configuration files that consist of "name=value" pairs grouped into sections. The syntax is similar to the INI-style formats that have been in user for a long time. (Specifically, the syntax is based on [gitconfig](http://git-scm.com/docs/git-config#_syntax), with minor changes.) Gcfg reads configuration data into a POGS (plain old Go structs). Gcfg also supports user-defined types and subsections.

Here is an example of a gcfg file:

```
; Comment line
[section]
name = value # Another comment
flag # implicit value for bool is true
```

The corresponding Go struct is:

```
type Config struct {
	Section struct {
		Name string
		Flag bool
	}
}
```

To read a file into a struct:

```
var cfg Config
err := gcfg.ReadFileInto(&cfg, "myconfig.gcfg")
```

See the [package documentation](http://godoc.org/code.google.com/p/gcfg) for usage details and more examples.

The recommended file extension for gcfg files is `.gcfg` .

## Where to start? ##

To install the library, run
```
go get code.google.com/p/gcfg
```

To update to the most recent version, run
```
go get -u code.google.com/p/gcfg
```

## Why gcfg? ##

The gcfg file format:
  * is simple and familiar to many users (based on git config)
  * aims for data portability (e.g. encoding is defined)

The gcfg library for Go:
  * supports reading gcfg file data into Go structs
    * struct definition can serve as (a base of) documentation for configuration items
  * allows parsing string values into user-defined types using fmt.Scanner

## Why not use an existing format? ##

The gcfg format is based on that for [git config](http://git-scm.com/docs/git-config#_syntax), which is presumably based on the various [INI file formats](http://en.wikipedia.org/wiki/INI_file) that have been in use for a very long time. The primary advantage of these styles compared to XML / JSON / YAML-based configuration is that the user does not have to mind tag, nesting and indentation consistency when editing the file. In addition, it encourages designing simpler configuration interfaces by not providing unlimited nesting capability.

Unfortunately, INI-style formats are often not well-defined. The git config syntax is well defined, easy to use and flexible, but it also has a few issues:
  * It treats strings as uninterpreted byte sequences, which can create portability and internationalization issues.
  * It allows importing arbitrary files from config files, which significantly complicates handling for questionable benefits, and can also create security and portability issues.
  * It does not require definitions for subsections and multi-valued variables to be contiguous, thus being prone to editing errors (e.g. mistakenly adding a extra value for a variable instead of changing the existing value).

Gcfg aims to overcome these issues. See the [package documentation](http://godoc.org/code.google.com/p/gcfg) for details.