# PowerShellのModuleをつくるよメモ
## ModulePath
"環境変数:`PSModulePath`" に任意のディレクトリを設定。

```
# 登録されているパスを一覧表示
$Env:PSModulePath -split ";"
```
> https://docs.microsoft.com/ja-jp/powershell/scripting/developer/module/modifying-the-psmodulepath-installation-path
> https://docs.microsoft.com/ja-jp/powershell/scripting/developer/module/importing-a-powershell-module

## WriteModule
### Moduleを定義する
- `${PSModulePath}/Hello` ディレクトリを作成。

Helloディレクトリ下にて、  
`Hello.psm1` ファイルを作成。  
※ ファイル名は**ディレクトリ名と同名**にすること。


- この時点で、Hello.psm1が空ファイルであってもロードが可能。
```
# Import可能なModuleのリストを表示
# `Hello` moduleも表示される
Get-Module -ListAvailable
```


- functionを記述。
```
# Hello.psm1
function Hello {
  Write-Output "Hello."
}
```


- 呼び出せる(もしかしたらImportが必要になる)。
```
# Import-Module Hello
Hello
```

### ModuleManifestの記述

> https://docs.microsoft.com/ja-jp/powershell/scripting/developer/module/how-to-write-a-powershell-module-manifest

## References
> https://tech.guitarrapc.com/entry/2013/12/03/014013#なぜモジュールを利用するのか
> https://forsenergy.com/ja-jp/windowspowershellhelp/html/3be86334-7efa-4ccd-952e-54afe47977a2.htm
