# PDS-2021-2-TP-Testes
Analisando 3 testes. TP de PDS. 

Feito por Raphael Heringer Rangel

# Testes Escolhidos:
https://github.com/sandiegopython/test-driven-django-development/blob/master/myblog/blog/tests.py
test-driven-django-development
Semestre passado usei o Django para realizar o TP de Engenharia de Software, por isso escolhi este repositório.

Este é um sistema para a criação de um Blog.

# Teste 1:
```
class HomePageTests(TestCase):

        def setUp(self):
                self.user = get_user_model().objects.create(username='some_user')

        def test_one_entry(self):
                Entry.objects.create(title='1-title', body='1-body', author=self.user)
                response = self.client.get('/')
                self.assertContains(response, '1-title')
                self.assertContains(response, '1-body')
                
        def test_two_entries(self):
                Entry.objects.create(title='1-title', body='1-body', author=self.user)
                Entry.objects.create(title='2-title', body='2-body', author=self.user)
                response = self.client.get('/')
                self.assertContains(response, '1-title')
                self.assertContains(response, '1-body')
                self.assertContains(response, '2-title')

        def test_no_entries(self):
                response = self.client.get('/')
                self.assertContains(response, 'No blog entries yet.')
```
Os testes verificam se os posts aparecem corretamente na Home Page.

Parte 1: Setp Up
- O teste cria um usuário apenas para realizar os seguintes testes.
- O próprio Django oferece models para o usuário (user).

Parte 2: 
- É testado se apenas 1 único Post aparece corretamente na Home Page.
- Primeiramente, ele cria um objeto da classe Entry (Post do Blog), com os 3 parâmetros necessários. Os parâmetros de data de publicação sao feitos automaticamente pelo sistema.
= Em seguida, ele faz o client receber a nova Entry.
- Em seguida, ele verifica se o que o client recebeu está com 2 parâmetros corretos (title e body). Como o parâmetro user é tratado pelo django automaticamente, não é preciso verificar se o user está correto.

Parte 3: 
- É testado se 2 Posts aparecem corretamente na Home Page. Como a inserção de Posts segue um processo automatizado, se 2 estiverem corretos, então n estão.
= Primeiramente, ele cria 2 objetos da Classe Entry.
= Em seguida, ele faz o client receber as duas novas Entry.
= Em seguida, ele verifica se o primeiro objeto possui title e body corretos.
= Em seguida, ele verifica apenas se o title do segundo objeto está correto. Se o primeiro estava com o body correto, então o segundo também estará. Era necessário apenas verificar se o segundo Entry estava presente na Home Page, afinal se a primeira Entry aparece corretamente, então as outras também aparecem, era necessário apenas saber se a próxima Entry estava presente.

# Teste 2

``` 
class CommentFormTest(TestCase):

        def setUp(self):
                self.user = get_user_model().objects.create_user('zoidberg')
                self.entry = Entry.objects.create(author=self.user, title="My entry title")
                
        def test_init(self):
                CommentForm(entry=self.entry)

        def test_url(self):
                title = "This is my test title"
                today = datetime.date.today()
                entry = Entry.objects.create(title=title, body="body",
                                             author=self.user)
                slug = slugify(title)
                url = "/{year}/{month}/{day}/{pk}-{slug}/".format(
                    year=today.year,
                    month=today.month,
                    day=today.day,
                    slug=slug,
                    pk=entry.pk,
                )
                response = self.client.get(url)
                self.assertEqual(response.status_code, 200)
                self.assertTemplateUsed(response, template_name='blog/entry_detail.html') 
```
Os testes verificam se as funcionalidades dos comentários, para um Post, funcionam.

Parte 1:
- É criado um Usuário e um Post, para que a seção de comentários sobre o Post possa ser testada.

Parte 2:
- É criado um Comentário.
- Um Comentário é uma Classe, assim como uma Entry.
- O próprio Django possui uma funcionalidade de tratar de Forms, e um Comentário é tratado por uma Form, por isso o código chamou um CommentForm.

Parte 3:
- Testa se a URL do comentário está funcionando.
- É preciso criar os parâmetros que compoem um objeto da classe Comment.
- Cria o formato da URL.
- O cliente recebe a URL.
- Verifica se o status é código 200, o que significa que o status está ok.
- Verifica se o Template utilizado para formatar o Comment está sendo utilizado corretamente.
