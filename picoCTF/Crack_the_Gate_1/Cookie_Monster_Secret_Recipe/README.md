# Cookie Monster Secret Recipe

起動すると以下の画面が表示される

![login_page](images/login_page.png)

とりあえず、ページソースを見たがなんもなさそうだった

次はdevtoolsを使う

タイトルを見ればわかるが、クッキーに関連した問題だと思うので、Cookieを見てみます。
![devtools](images/devtools.png)

これは怪しそうですね
```
secret_recipe:"cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzX0M0MzBBRTIwfQ%3D%3D"
```
```%3D%3D```これ特に注目した
これは```%```+16進数2桁の組み合わせのためなんかエンコードしたか？と思った

それについて調べてみたところURLエンコードで```=```は```%3D```に変換されることを理解した

ならば末尾に```==```がつくということが理解できるため
これは更にbase64もエンコードもされてるだろうなと考えた

```
flang
↓
base64エンコード
↓
URLエンコード
↓
secret_recipe:"cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzX0M0MzBBRTIwfQ%3D%3D"
```
ならば

```
secret_recipe:"cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzX0M0MzBBRTIwfQ%3D%3D"
↓
URLデコード
↓
base64デコード
↓
flag

```
というプロセスをたどればいいわけだ

```javascript
atob(decodeURIComponent("cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzX0M0MzBBRTIwfQ%3D%3D"));
"picoCTF{c00k1e_m0nster_l0ves_c00kies_C430AE20}"
```

