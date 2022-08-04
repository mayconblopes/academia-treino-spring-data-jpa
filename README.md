<h1> Projeto Spring Data JPA na Prática </h1>

<h2>🎯 Objetivo do Projeto</h2>
<p>Treinar os principais conceitos de mapeamento objeto relacional (ORM) usando o <strong>Spring Data JPA</strong>. Para isso, uma <strong>API RESTful</strong> foi desenvolvida com ênfase na modelagem de suas entidades, no domínio de uma academia de ginástica.</p>

<h2>
   Requisitos 
</h2>

- [x] Fundamentos do Spring Boot

- [x] Noções de SQL

<h2> 🚦 Guia </h2>

<ol>
    <li> Apresentação do Projeto </li>
    <li> Configuração do banco de dados (SGBD <em>PostgreSQL</em>)</li>
    <li> Aplicando as <em>annotations</em></li>
    <li>Execução do fluxo back-end: <em>Controller - Service - Repository</em></li>
</ol>

<h2>🛠 Apresentação do Projeto</h2>

<h3> As tecnologias utilizadas são as seguintes: </h3>

<ul>
    <li>IDE IntelliJ</li>
    <li>Java 11</li>
    <li>Maven</li>
    <li><strong>Spring Web</strong></li>
    <li><strong>Spring Data JPA</strong></li>
    <li><strong>PostgreSQL Driver</strong></li>
    <li><strong>Hibernate Validator</strong></li>
    <li>Lombok</li>
    <li>Postman</li>
</ul>

<h3> Diagrama ER BD </h3>
<img src="https://i.postimg.cc/7PdxpQhs/Diagrama-ERDB.png" />

<h3> Fluxo Back-End </h3>
<img src="https://i.postimg.cc/65ZGSZWZ/Fluxo-Back-end.png" />


<h2>🛠 Passos na configuração do Banco de Dados (SGBD <em>PostgreSQL</em>)</h2>
<ol>
    <li>Renomeei o arquivo application.properties para application.yml e nele acrescentei as configurações do banco de dados Postgres e JPA com Hibernate.</li>
    <li>Criei o banco de dados 'academia' no PostgreSQL.</li>
    <li>Comecei a editar o model (ou entity) 'Aluno', para acrescentar as anotações a fim de gerar as tabelas, mas comecei a ter problemas com dependências do maven (erro 'cannot resolve symbol'). Para solucionar, atualizei o repositório nas configurações do IntelliJ (Build, Execution, Deployment > Build Tools > Maven > Repositories) e executei o comando 'mvn dependency:resolve'.</li>
    <li>Ainda editando o model Aluno, configurei um atributo 'id' com as anotações @Id e @GeneratedValue; o atributo CPF defini como único, usando @Column(unique=true); fiz um relacionamento com @OneToMany entre os models Aluno e AvaliacaoFisica.</li>
    <li>Repeti as configurações aplicáveis nos models 'AvaliacaoFisica' e 'Matricula'. Vide código para detalhes.</li>
</ol>

<p>As anotações de mapeamento usadas estão detalhadas abaixo:</p>
<h2><a href="https://strn.com.br/artigos/2018/12/11/todas-as-anota%C3%A7%C3%B5es-do-jpa-anota%C3%A7%C3%B5es-de-mapeamento/"> Anotações de Mapeamento </a></h2>

<strong>@Entity</strong>
Usada para especificar que a classe anotada atualmente representa um tipo de entidade.

<strong>@Table</strong>
Usada para especificar a tabela principal da entidade atualmente anotada.

<strong>@Id</strong>
Especifica o identificador da entidade. Uma entidade deve sempre ter um atributo identificado.

<strong>@GeneratedValue</strong>
Especifica que o valor do identificador de entidade é gerado automaticamente.

<strong>@Column</strong>
Usada para especificar o mapeamento entre um atributo de entidade básico e a coluna da tabela de banco de dados.

<strong>@JoinColumn</strong>
Usada para especificar a coluna FOREIGN KEY. Indica que a entidade é a responsável pelo relacionamento.

<strong>@OneToMany</strong>
Usada para especificar um relacionamento de banco de dados um-para-muitos.

<strong>@OneToOne</strong>
Usada para especificar um relacionamento de banco de dados um-para-um.

<strong>@ManyToOne</strong>
Usada para especificar um relacionamento de banco de dados muitos-para-um.

<strong>cascade</strong>
Realizar operações em cascata só faz sentido em relacionamentos Pai - Filho.

<strong>mappedBy</strong>
Indica qual é o lado inverso ou não dominante da relação.

<h2>🛠 Passos na execução do fluxo <em>Back-end: controller - service - respository</em>)</h2>
<ol>
<li>Na controller 'AlunoController', fiz a injeção do service 'AlunoServiceImpl' com a anotação @Autowired;</li> 
<li>Para retornar a lista de alunos, em 'AlunoController' criei o método getAll() usando @GetMapping, que na verdade chama o método getAll() do service;</li>
<li>Como a lógica para retornar todos os alunos fica no service, foi necessário implementar o método getAll() em 'AlunoServiceImpl';</li>
<li>Criei então o respository 'AlunoRespository', extendendo de JpaRepository, para assim conseguir implementar o método getAll() usando o 'repository.findAll()';</li>
<li>Procedi para criação de um novo método chamado create em 'AlunoController' para criar aluno, usando anotação @PostMapping. O método create delega a lógica para o service 'AlunoServiceImpl' que instancia um novo aluno e setta seus atributos com base nas informações do formulário 'AlunoForm';</li>
<li>Com o método create pronto, realizei os testes das requisições Http via Postman, verificando que estava funcionando como esperado;</li>
<li>Procedi então para repetir os passos em 'AvaliacaoFisicaController', desde a injeção de um novo service (@Autowired) do tipo 'AvaliacaoFisicaServiceImpl' até a implementação do respectivo método create;</li>
<li>Contudo, ao pegar a lista de alunos (localhost/alunos), as suas avaliações físicas não estavam sendo exibidas. Para solucionar isso, criei um método 'getAllAvaliacaoFisica()' na interface 'IAlunoService' que retorna uma lista de 'AvalicaoFisica';</li>
</ol>

<img src="https://i.postimg.cc/MZ0sW92D/Postman-Get-lista-alunos.png" />
<img src="https://i.postimg.cc/tCXB7F9H/Postman-Post-cria-aluno.png" />
<img src="https://i.postimg.cc/kXffSD5R/Postman-Post-cria-avaliacao.png" />

<h2>🔗 Links Úteis</h2>
<ul>
    <li><a href="https://start.spring.io/#!type=maven-project&language=java&platformVersion=2.6.1&packaging=jar&jvmVersion=11&groupId=me.dio.academia&artifactId=academia-digital&name=academia-digital&description=Tutorial%20API%20RESTful%20modelando%20sistema%20de%20academia%20de%20gin%C3%A1stica&packageName=me.dio.academia.digital&dependencies=web,data-jpa,postgresql,validation,lombok">Spring Initializr</a></li>
    <li><a href="https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/common-application-properties.html">Common application properties</a></li>
    <li><a href="https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.repositories">Spring Data JPA - Reference Documentation</a></li>
</ul>


------------

Crédito pelo projeto base: [cami-la](https://www.linkedin.com/in/cami-la/ "cami-la").




