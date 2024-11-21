---
layout: page
title: lets-go
description: Simple CLI application written in Go for encrypting and decrypting files 
img: assets/img/lets-go/lets-go-1.png
importance: 2
category: security
---

## Description 
In this project I wanted to apply some cryptography knowledge I had learned during the first semester of my master's degree. My goal was to make a CLI program which could easily encrypt and decrypt files. This way, I could review some of my knowledge around (a)symmetric encryption algorithms and also gain more familiarity with the Go programming language.

## Overview
I started by creating the base program of all learning journeys, which is a "hello-world" program. I quickly realized that my Go knowledge was very limited, so I consulted some online resources such as [this video](https://www.youtube.com/watch?v=8uiZC0l4Ajw&ab_channel=AlexMux), which gives a great overview of Go if you're already familiar with another programming language.

After adding the basic features I had in mind, I created a Makefile to streamline the build and test process a bit. After some final adjustments, I was able to encrypt and decrypt files like I had planned.

```bash
$ make
GOOS=windows GOARCH=amd64 go build -o bin/lets-go-windows.exe cmd/main.go
GOOS=linux GOARCH=amd64 go build -o bin/lets-go-linux cmd/main.go
GOOS=darwin GOARCH=amd64 go build -o bin/lets-go-macos cmd/main.go
$ cat test.txt
This is a test file. Encrypt me please!
$ ./bin/lets-go-linux encrypt test.txt 
Enter passphrase: my-secret-passphrase
Verify passphrase: my-secret-passphrase
File encrypted successfully
$ cat test.txt.enc 
K�z]�
     Y��$A+���Tp����f5�ѵ����_�{ٵO>��i�i�X)��zS�c�ŀ��o1�%                                                                                                                                        
$ rm test.txt
$ ./bin/lets-go-linux decrypt test.txt.enc 
Enter passphrase: my-secret-passphrase
Verify passphrase: my-secret-passphrase
File decrypted successfully
$ cat test.txt
This is a test file. Encrypt me please!
```

## Result
For now I reached my goal of creating the simple CLI application I had in mind. On the GitHub repository I wrote down some features I would like to add in the future, mainly to improve my skills with Go and to improve the usability of the CLI tool. Have a look [here](https://github.com/cadeke/lets-go?tab=readme-ov-file#future-improvements) if you're interested what features I have planned.

## References
- Thumbnail source: [GitHub](https://github.com/MariaLetta/free-gophers-pack)
- GitHub repository: [lets-go](https://github.com/cadeke/lets-go)
