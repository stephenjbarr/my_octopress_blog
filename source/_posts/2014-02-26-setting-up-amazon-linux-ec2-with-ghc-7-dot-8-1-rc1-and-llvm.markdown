---
layout: post
title: "Setting up Amazon Linux (EC2) with GHC 7.8.1 RC1 and LLVM"
date: 2014-02-26 11:41
comments: true
categories: [haskell, ec2, amazon, llvm, hpc]
---

GHC 7.8.1 RC1 has been [recently released](http://www.well-typed.com/blog/85), and there are [many interesting changes](http://www.haskell.org/ghc/docs/7.8.1-rc1/html/users_guide/release-7-8-1.html). Among them is preliminary [support for SIMD](https://ghc.haskell.org/trac/ghc/wiki/SIMD) using [Haskell's llvm backend](https://ghc.haskell.org/trac/ghc/wiki/Commentary/Compiler/Backends/LLVM). I have recently working on a dynamic program solver for an inventory research problem. I want to solve the problem over a large number of parameterizations of the problem, so program execution speed is of key importance. LLVM shines in this area, by [optimizing the generated assembly code to take advantage of the program structure](http://donsbot.wordpress.com/2010/02/21/smoking-fast-haskell-code-using-ghcs-new-llvm-codegen/), such as unrolling loops.

As part of this, it would be great to run GHC 7.8.1 and LLVM on [Amazon's EC2](http://aws.amazon.com/ec2/). Their 3rd generation cluster compute instances (cc3.x) are particularly appealing because they have Intel Xeon E5-26xx v2 processors. This means wide AVX2 registers. However, these instances only run Amazon's linux distribution, which is a derivative of Fedora. I typically use Arch linux, and getting things setup is easy. For Amazon linux, it was a bit more involved. This blog post is to document what I have done. And, if someone saves an hour or two by reading this post, the world is probably (marginally) better off.

Start your instance with this command, replacing key, region, security group to match your preferences. The important thing is to have some ephemeral storage to make room for the compiles.
{% codeblock %}
ec2-run-instances --region us-west-2 ami-ccf297fc -k yourkey -t "c3.4xlarge"  -g your-security-group -b "/dev/sdb=ephemeral0" -b "/dev/sdc=ephemeral1"
 {% endcodeblock %}


The script does the following:

* Update packages, install gcc, zlib, freeglut.
* Download, compile, and install latest llvm.
* Install libgmp (note, gmp-devel package does not have libgmp.so.10, which is needed by ghc).
* Install new glibc (need version 2.15). This compile takes some time.
* Download the precompiled GHC 7.8.1 RC binaries, configure and install.
* Download newest Cabal (currently 1.19) build and install cabal library and cabal binary.
* cabal update and cabal install vector.

Note, I would make sure to do these steps by hand, following the script by hand. Alternatively, you may use my AMI: ami-4e771b7e, in the us-west-2 region.

<script src="https://gist.github.com/stephenjbarr/9237286.js"></script>

