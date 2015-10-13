# No Mac OSX Usando Homebrew

Para instalar facilmente o Crystall no seu Mac, você pode usar o nosso [tap](https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/brew-tap.md) do [Homebrew](http://brew.sh/).

```
brew tap manastech/crystal
brew update
brew install crystal-lang
```

Se você planeja contribuir com o projeto, você pode achar útil também instalar o LLVM, então substitua a última linha por:

```
brew install crystal-lang --with-llvm
```
