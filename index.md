---
layout: page
title: Indis
tagline: The intelligent disassembly framework
---
{% include JB/setup %}

Indis is an intelligent disassembly framework. The primary goal for indis is to provide facilities for analysis of binary files. The plan is to provide a highly extensible object model for binary contents with support of reversing back up to C-like code.

Indis consists of a [core](http://github.com/indis/indis-core) gem, that unifies all the different target formats and architectures (which is a pretty hard task for now, when I'm focused on Mach-O ARM targets only); format processing gems (like [Mach-O](http://github.com/indis/indis-macho)), that provide support of loading the specific binary format and collecting generic info about it (stuff like virtual map of sections, symbol references, etc.); and cpu processing gems ([ARM](http://github.com/indis/indis-arm) for now).

### Source Code

You can find the gems source at [GitHub](http://github.com/indis).

### API Documentation

 - [indis-core](http://rdoc.info/github/indis/indis-core/frames)
 - [indis-macho](http://rdoc.info/github/indis/indis-macho/frames)
 - [indis-arm](http://rdoc.info/github/indis/indis-arm/frames)

### Updates

See the [archive](/archive.html) for updates on the project.

### Build Status

#### Core
[![Build Status](https://secure.travis-ci.org/indis/indis-core.png?branch=master)](http://travis-ci.org/indis/indis-core)

#### Mach-O
[![Build Status](https://secure.travis-ci.org/indis/indis-macho.png?branch=master)](http://travis-ci.org/indis/indis-macho)

#### ARM
[![Build Status](https://secure.travis-ci.org/indis/indis-arm.png?branch=master)](http://travis-ci.org/indis/indis-arm)

### License

This project is distributed under the terms of [GPL-3](http://www.gnu.org/licenses/gpl.html) license.