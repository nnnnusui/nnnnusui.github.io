
## ModulePath
"環境変数:`PSModulePath`" に任意のディレクトリを設定。

```
# 登録されているパスを一覧表示
$Env:PSModulePath -split ";"
```

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
