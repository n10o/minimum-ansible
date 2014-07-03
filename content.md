# 小さく始めるAnsible
～ ansibleが出てくるのは12ページ目から ～

---

## 複数台サーバの設定変更
## どうやってますか？

---

## ありがちな作業
1. ソフトのインストール
1. ソフトの設定変更
    - ホストによって設定内容が違う
        - リライト先の設定
        - 最大アップロード可能サイズ
    - サービス再起動

---

## ありがちな作業を手でやると
- ３台超えたぐらいからしんどい
    - １、３の法則 (１台、３台、１０台、３０台)
- 手作業によるミス
- 途中からわけわからなくなる

<img src="image/despair.jpg" style="width: 700px; height: 400px;"/>
 
---

## 自動化して楽になりたい

--

<!-- .slide: data-background="#DDDDDD" -->
# でも

---

## chefを導入していない
### もうシステムは動いている
### そんなに規模増えないし
### そんな工数かけられない
## 腰が重い

---

## 結論１：自動化は悪
## 新たな雇用を生み出そう！

--

<!-- .slide: data-background="#33CC99" -->
## イギリスの産業革命
- 工場制機械工業の導入により職を追われる人が急増
- 手作業によりシステムに温かみが生まれる
- 職人芸の尊重
- 自動化すればいいというものではない

<img src="image/industrialrevolution.jpg" style="width: 800px; height: 400px;"/>

---

## 雇用を生み出す余裕がない場合
### デプロイツールで簡単コマンド実行

---

## デプロイツール

複数サーバに並列でsshをつないでよしなにコマンド実行

別にデプロイしなくてもいい

- capistrano (Ruby)
    - Remote multi-server automation tool
- pssh, fabric (Python)
- cinnamon (Perl)

---

## Capistrano Example
### my_secret_dirを複数ホストに作る

```
$ cat capfile
role :web, "192.168.1.110", "192.168.1.120"
task :secretdir, :roles => :web do
  run "mkdir my_secret_dir"
end

$ cap secretdir
```

---

## 気軽に使うならこれで十分

<img src="image/happy.jpg" style="width: 300px; height: 500px;"/>

---

## 結論２：デプロイツールで解決
### これが簡単な解決方法

---

## サーバの構成管理もやりたい
### 構成管理ツールを導入
#### サーバ内の設定とサーバ間の構成をコード化して管理

- chef, puppet (Ruby)
- ansible (Python) 

---

# やっと登場
# ansible
### ～ 今度はしばらくchefの話になります ～

---

<img src="image/chef.jpg" style="width: 200px; height: 300px;"/>

## 構成管理ってハードル高そう
### chef-serverはハードル高いらしい
### そこでchef-solo

---

# chef-solo
- ホスト1台に限定
  - chefブーム到来
      - vagrantと組み合わせて自分のPC内にLinuxマシンを簡単導入+自動セッティングみたいな
  - ハードルの低さ重要
- chef-soloをもっと簡単に使えるようにしたものがknife-solo

---

## 1台だけじゃつらい
### chef-soloで複数ホスト実行をうまくやるためには一工夫必要
#### (例：5台のfluentd設定ファイル書換え)
- Capistranoでchef-soloをキック (Ruby x Rubyの黄金コンビ)
    - Roundsman
- Knife Sous

---

## だんだんわけわからなくなってきましたか？
### = 始める前に覚えることが多い

<img src="image/grief.jpg" style="width: 700px; height: 400px;"/>

---

<!-- .slide: data-background="#33CC99" -->
## そこでansible
### Ansible is simplest way to automate IT.

---

<!-- .slide: data-background="#FF9933" -->
## どれだけシンプルか
1. yum install ansible ( from epel )
1. 接続先にssh接続できること
1. やりたい処理を書く
1. Enjoy!

---

# 処理の書き方
## サーバ名一覧を準備
```
$ cat hosts
[webservers]
192.168.1.110
taro
jiro[1:100]
```
- jiro[1:100]とすればjiro1からjiro100まで実行可能

---

# dir作成
- サーバ名一覧を-iで与えて-aにコマンド書く
```
ansible -i hosts all -a 'mkdir my_secret_dir'
```

---

# これであなたも

---

# アンシブラー

---

## デプロイツール的にゆるく活用
### もっと本格的に
### 構成管理したくなったらコード化

---

# playbook
## 作業内容をコード化
```
$ cat mkdir.yml
- hosts: webservers
  tasks:
   - name: create my poem directory
     file: dest=my_secret_dir state=directory

$ ansible-playbook -i hosts mkdir.yml

```

---

## 実運用の困りポイント
- 規模が大きくなるとplaybookを分割しないときつい
- 開発環境と本番環境で設定が違う
- Front-WebサーバとAdmin-WebサーバとBackendサーバで共通な設定もあれば、共通でない設定も
- 複数プロジェクトを管理する場合の設定

---

## 管理方法の設計が必要
- ディレクトリを"適切に"分割して
- その中でplaybookを"適切に"配置して
- 変数部分を"適切な"ファイルに設定して

<img src="image/despair2.jpg" style="width: 700px; height: 400px;"/>

---

# 設計
# = ハードル高い

---

## ベストプラクティス
### ディレクトリ/ファイル構成をこんな感じにすれば大体いい感じになる構成例
- [ansible公式HP](http://docs.ansible.com/playbooks_best_practices.html)で公開
- 凄まじく便利
- 万能ではない
- そこで

---

# ムネオプラクティス
- ムネオ = 私
- ベストプラクティスを独自カスタマイズ
- ムラクティス

--

<!-- .slide: data-background="#007777" -->
# 近日

--

# 公開

--

<!-- .slide: data-background="#DDDDDD" --> 
##### 予定

---

<!-- .slide: data-background="#66CCFF" -->
# Ansible デモ
### Fluentd設定書換
### ＋ サービス再起動

---

## chefとansibleの違い
- できることは大体同じ
    - どちらも日々進化しており、良いライバル関係（妄想）
- chefはドキュメント豊富でWebに情報多い(印象)
- ansibleはインストールや設定が簡単で、始めるハードル低い
    - 5000台の運用実績

---

## ansibleから初めて、
## 不満があれば
## chefを使ってみては？

---

# おわり

---

# おまけ
- プレゼン資料はreveal.js+markdown+github pagesで作成しています。
- [ここ](https://github.com/n10o/minimum-ansible)でソースを公開しているので・・・
