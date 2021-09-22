<h1 align="center">API Lances de Leilão</h1>

<div align="center">

[![Status](https://img.shields.io/badge/status-active-success.svg)]()

</div>

---

<p align="center"> Projeto de lances de Leilão feito em JAVA
    <br> 
</p>

## 📝 SUMÁRIO

- [Sobre o Projeto](#about)
- [Iniciando o Projeto](#getting_started)
- [Testes](#tests)
- [Ferramentas Utilizadas](#built_using)
- [Autor](#authors)

## 🧐 SOBRE O PROJETO <a name = "about"></a>

API lances de leilões é um modelo de cadastro para leilões online em JAVA, utilizando CRUD, interface web e Banco de Dados.

## 🏁 INICIANDO O PROJETO <a name = "getting_started"></a>

Faça o download do projeto e rode via terminal ou em sua IDE de preferência.

### REQUISITOS

- GIT;
- JAVA 16;
- VS CODE OU IDE (ECLIPSE OU INTELLIJ)
- JUNIT (PARA TESTES - JÁ IMPLEMENTADO DENTRO DO PROJETO)

## 🔧 TESTES <a name = "tests"></a>

Os testes de integração foram implementados utilizando as classes <b>DAO</b> e automatizado com padrão da pirâmide chamado Services, integrando junto ao BD.

### IMPLEMENTANDO OS TESTES

Os Testes estão dentro do pacote chamado "tests" do projeto.

Foram utilzados para criar os testes as classes <b>DAO</b> do projeto e assim padronizar conforme as boas práticas.

O Padrão DAO (DATA ACCESS OBJECT) é um padrão de comunicação com Banco de Dados, por isso possui algumas anotações da JPA para que o BD reconheça o que a classe está gerando. Para o teste, não utilizarei as anotações de teste da JPA.

Para que possamos melhorar a performance e manutenção a longo prazo do teste, utilizamos o @BeforeEach e @AfterEach, fazendo com que o código seja mais legível e gere menos desgate na hora da manutenção 

```
@BeforeEach
	public void beforeEach() {
		this.em = JPAUtil.getEntityManager();
		this.dao = new LeilaoDao(em);
		em.getTransaction().begin();
	}

	@AfterEach
	public void afterEach() {
		em.getTransaction().rollback();
	}
```
Para Cada Teste dentro da classe DAO, utilizarei o padrão Builder, que fará com que apenas implemente os métodos, sem que precisemos utilizar mais código e tenha maior dificuldade de manutenção.

```

  @Test
	void deveriaEncontrarUsuarioCadastrado() {
		Usuario usuario = new UsuarioBuilder()
			.comNome("Fulano")
			.comEmail("fulano@email.com")
			.comSenha("12345678")
			.criar();
		em.persist(usuario);
		
		Usuario encontrado = this.dao.buscarPorUsername(usuario.getNome());
		Assert.assertNotNull(encontrado);
	}

	@Test
	void naoDeveriaEncontrarUsuarioNaoCadastrado() {
		Usuario usuario = new UsuarioBuilder()
			.comNome("Fulano")
			.comEmail("fulano@email.com")
			.comSenha("12345678")
			.criar();
		em.persist(usuario);

		Assert.assertThrows(NoResultException.class, () -> this.dao.buscarPorUsername("beltrano"));
	}

	@Test
	void deveriaRemoverUmUsuario() {
		Usuario usuario = new UsuarioBuilder()
			.comNome("Fulano")
			.comEmail("fulano@email.com")
			.comSenha("12345678")
			.criar();
		
		em.persist(usuario);
		dao.deletar(usuario);

		Assert.assertThrows(
			NoResultException.class, 
			() -> this.dao.buscarPorUsername(usuario.getNome())
		);
	}

```
## ⛏️ Ferramentas Utilizadas <a name = "built_using"></a>

- [GIT](https://git-scm.com/) - Versionamento de Código
- [H2](https://www.h2database.com/html/main.html) - Database
- [Java 16](https://www.oracle.com/java/technologies/downloads/#java16) - Linguagem Utilizada
- [Editor de Texto](https://code.visualstudio.com/) - Editor de Texto

## ✍️ Autor <a name = "authors"></a>

- [@andersonVallada](https://github.com/andersonVallada)
