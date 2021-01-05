# Python

pythonはパッケージ管理だけ

## poetry

rubyで言う所のbundleに近いが、記述形式はtomlなのでrustのcargoにも似ている。pipenvの方が公式感あるけど、こちらの方がシンパシーを感じるのでこれを採用。

(setup.py, requirements.txt, setup.cfg, MANIFEST.in) などを pyproject.toml に置き換える。

```sh
poetry self update # poetry自体を最新版にアップデート

poetry init # pyproject.tomlの新規作成
poetry add oslo.utils=1.4.0
poetry install || # pyproject.tomlに基づきインストール
poetry update # pyproject.tomlに基づきインストール、lock更新
poetry run # pythonの実行
poetry shell # 仮想環境下でシェルの実行
poetry run python -c 'import fn_reflection' # 仮想環境下でpythonをコマンドライン実行
```

