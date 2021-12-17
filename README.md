# M1 Macでbcryptを使う方法

## M1のnode.jsをインストールする（失敗パターン）

ぼくは node.js のバージョンマネージャーで asdf を使っているので、

`asdf` で node.js v12.16.1 をインストールします。

```
asdf install nodejs 12.16.1
```

`yarn init -y` したディレクトリで、 `yarn add bcrypt` をしました。

### bcryptのインストールが失敗する

以下のようなエラーが発生しました。

```
 hisasann  ~/_/j/bcrypt-m1-install  node index.js                                                                                                          26.8s  火 12/14 01:05:14 2021
node:internal/modules/cjs/loader:1179
  return process.dlopen(module, path.toNamespacedPath(filename));
                 ^

Error: dlopen(/Users/user/_/js-code/bcrypt-m1-install/node_modules/bcrypt/lib/binding/napi-v3/bcrypt_lib.node, 0x0001): tried: '/Users/user/_/js-code/bcrypt-m1-install/node_modules/bcrypt/lib/binding/napi-v3/bcrypt_lib.node' (mach-o file, but is an incompatible architecture (have 'x86_64', need 'arm64e')), '/usr/local/lib/bcrypt_lib.node' (no such file), '/usr/lib/bcrypt_lib.node' (no such file)
```

## Intelのnode.jsをインストールする（成功パターン）

### iTerm2をRosettaで開く

「右クリック -> 情報」を開いて、 `Rosettaを使用して開く` にチェックを入れてから iTerm2 を起動します。

M1 ではなく Intel で node.js をインストールし、 bcrypt も Interl モードでインストールします。

```
asdf uninstall nodejs 12.16.1
arch -x86_64 asdf install nodejs 12.16.1
yarn add bcrypt
```

```
node index.js
```

これでこのコードがコケずに実行できます。

## インストール後はRosettaのチェックを外しても成功する

インストールまでうまくいけば問題ないので、そこまで来たら iTerm2 の Rosetta のチェックを外して起動して、

```
node index.js
```

しても大丈夫でした。

## まとめ

今回は、 `bcrypt` のパターンでしたが、 `node-gyp` や `node-sass` なども繊細でバージョンによっては Intel 版で動かさないとビルドが通らないなどもあったので、古いプロダクトの場合は Rosetta で開いて、インストールまで行い、実際に実行するときは M1（arm）で動かすというのもありかもしれません。
