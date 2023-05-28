# Ansible Role - 作成用テンプレ

Ansible Galaxy に公開しないRoleをローカル開発する際のテンプレートです。

開発環境は macOS + Docker or Lima + Visual Studio Code (Remote - Containers) + Molecule の環境を想定しています。

# 開発環境

Visual Studio Codeのdevcontainer機能が利用できます。
(docker / docker compose が利用できる環境である必要があります)

MuleculeによるRoleテスト環境を利用するためには、
devcontainer上から `docker.sock` にアクセスできる必要があります。

この情報をコンテナに渡すための設定として`.devcontainer/.env`に設定が必要ですので、
devcontainer起動前にこれを設定してください。

```sh
cp .devcontainer/.env.sample .devcontainer/.env
## 編集
vi .devcontainer/.env
```

`.devcontainer/.env` の内容は下記のとおりです。

```sh
## DOCKER_SOCKET_PATH - docker.sock をマウントするための環境変数を指定します。
DOCKER_SOCKET_PATH=/var/run/docker.sock

## Limaなどを使用している場合、macOS上に露出したdocker.sockのパスではなく、
## Dockerが実際に動作するVM上のファイルパスを指定する必要があります。
## 例えば、macOS上でLimaを使用し、rootlessモードのDockerを使用している場合は、
## 下記の場所にdocker.sockが存在します。
# DOCKER_SOCKET_PATH=/run/user/501/docker.sock

## DOCKER_SOCKET_GID - docker.sock のオーナーGroupのGID
## コンテナ上のユーザからdocker.sockにアクセスするために、docker.sockの
## オーナーグループのGIDを指定します。
## Limaなどを使用している場合、macOS上に露出したdocker.sockのパスではなく、
## Dockerが実際に動作するVM上のパーミッションを参考にGIDを指定する必要があります。
##   lima ls -l /
DOCKER_SOCKET_GID=999
```

リモートコンテナ上で起動が完了したら、テスト環境を下記の通り起動できます。

```sh
# ターゲットコンテナを作成します
molecule create
# ターゲットコンテナにAnsibleを実行します
molecule converge
# ターゲットコンテナ上で実行結果をテストします
molecule verify
# ターゲットコンテナを削除します
molecule destroy
```
