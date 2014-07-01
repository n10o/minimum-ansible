# 小さく始めるAnsible
～ ansibleが出てくるのは20ページ目から ～

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

---

## 自動化して楽になりたい

--

<!-- .slide: data-background="#000000" -->
# でも

---

## chefを導入していない
### もうシステムは動いている
### そんなに規模増えないし
### そんな工数かけられない

---

## 結論１：自動化は悪
## 新たな雇用を生み出そう！

--

<!-- .slide: data-background="#007777" -->
## イギリスの産業革命を思い出そう
- 工場制機械工業の導入により職を追われる人が急増
- 手作業によりシステムに温かみが生まれる
- 職人芸の尊重

---

## 新たな雇用を生み出す余裕がない場合
### デプロイツールで簡単コマンド実行

---

## デプロイツール =
## Remote multi-server automation tool
複数サーバに並列でsshをつないでよしなにコマンド実行

- capistrano (Ruby)
- pssh, fabric (Python)
- cinnamon (Perl)

---

## 気軽に使うならこれで十分

---

## 結論２：デプロイツールで解決
### これが簡単な解決方法

---

## サーバの構成管理もやりたい
### 構成管理ツールを導入
#### サーバ内の設定とサーバ間の構成をコード化して管理

- chef, puppet (Ruby)
- やっと登場 ansible (Python) 

---

## 構成管理ってハードル高そう
### chef-serverはハードル高いとの声がWebには多数
### chef-soloがお気軽簡単で、chefブーム到来

### chef-soloをもっと簡単に使えるようにしたものがknife-solo

---

## chef-soloで複数ホスト実行をうまくやるためには一工夫必要
### (例：10台のfluentd設定ファイル書き換え)
- Capistranoでchef-soloをキック (Ruby x Rubyの黄金コンビ)
    - Roundsman
- Knife Sous

---

## だんだんわけわからなくなってきましたか？
### = 始める前に覚えることが多い

---

## そこでansible
### Ansible is simplest way to automate IT.

---

## どれだけシンプルか
1. yum install ansible (from epel)
1. 接続先にssh接続できること
1. やりたい処理を書く
1. Enjoy!

---

# Ansible デモ

---

システム構成 5台から送る

## サーバ名一覧を準備
```
$ cat hosts
[webservers]
12.123.234.56
taro
jiro[1:100]
```

---

## 実運用の困りポイント
- 開発環境と本番環境を分割
- Front-WebサーバとAdmin-WebサーバとBackendサーバで共通な設定もあれば、共通でない設定も
- 複数プロジェクトを管理する場合の設定

---

## ansibleでどう管理するか設計が必要

---

## ベストプラクティス =
### ディレクトリ/ファイル構成をこんな感じにすれば大体いい感じになる構成例
- [ansible公式HP](http://docs.ansible.com/playbooks_best_practices.html)参照
- 万能ではない

---

## ムネオプラクティス
- ムネオ = 私
- ベストプラクティスを独自カスタマイズ
- ムラクティス

--

<!-- .slide: data-background="#007777" -->
# 近日

--

# 公開！

--

<!-- .slide: data-background="#000000" --> 
##### 予定

---

## chefとansibleの違い
- できることは大体同じ
    - どちらも日々進化してるし、良いライバル関係（勝手な妄想）
- chefはドキュメント豊富なので小回り効きそう（な印象）
    - Webに情報も多い
- ansibleはインストールや設定が簡単で、始めるハードル低い

---

## ansibleから初めて、不満を感じたらchefを使ってみては？
### ansibleもサーバ5000台の運用実績

---

# おわり

---

# Conclusion
- This presentation made by reveal.js and hosted by github pages.
- If you like this, give me [Star](https://github.com/n10o/minimum-ansible) please.
