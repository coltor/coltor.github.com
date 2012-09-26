---  
layout: post
title: PE File Checksum Algorithm    
---
这两天闲来无事，在看PE文件相关的文章，我们知道PE文件是有校验和算法的，而且像驱动，或者系统DLL之类的特殊执行文件，对校验和的要求是极其高的，如果没算对，就认为是非法文件。微软SDK里提供一个API，叫MapFileAndCheckSum，这个可以计算出一个PE文件的校验和，该API的相关分析，可以参考这篇文章[《An Analysis of the Windows PE Checksum Algorithm》](http://www.codeproject.com/KB/cpp/PEChecksum.aspx)。关于校验和算法，还有一个比较有意思的算法是IP协议的校验和算法，可以看[RFC 1071](http://datatracker.ietf.org/doc/rfc1071/?include_text=1)。还有WIKI中的这篇文章[《Error detection and correction》](http://en.wikipedia.org/wiki/Redundancy_check#Error_detection_schemes)。  
我感觉这个应用层实现的PE CHECK SUM算法太过恶心了..就看了下系统内核PE加载器的内核实现，发现校验和算法部分和应用层的完全不同，比应用层的简洁多了，直接上代码:  

<script src="https://gist.github.com/3747774.js"> </script>