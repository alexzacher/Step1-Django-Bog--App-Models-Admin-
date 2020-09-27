# Django primeiros passos Blog
=+=+=+==+=+=+==+=+=+==+=+=+==+=+=+==+=+=+==+=+=+=
[I'm an inline-style link](https://www.google.com)

~~Scratch this.~~

*asterisks*

__underscores__

⋅⋅* Unordered sub-list.


```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```
> Blockquotes are very handy in email to emulate reply text.

---

=+=+=+==+=+=+==+=+=+==+=+=+==+=+=+==+=+=+==+=+=+=

## Criação de Base de dados, admin e models e seu Admin.

#### 1
   * Pip install Django
   * Pip install pillow
   * Pip install mysqlclient

#### 2
Iniciando o projeto
   * Django-admin startproject blog .  <( ponto no final é necessario para nao acriar duas pastas )
Esta sera a pasta Mean do programa.

#### 3
Criando os apps
Craicao de sub pastas, para a divisao de seu site. Entre no terminal e digite.

Categorias
   * python manage.py startapp categorias
Post
   * python manage.py startapp posts
Comentarios
   * python manage.py startapp comentarios

Setting da pasta principal blog - INSTALLED_APPS

Adicione nas primeiras linhas como 
‘categotias’ , 
‘posts’ ,
‘comentarios’ , 

4 passo
import os






