# O que é Spring MVC?
Repositorio do curso "O QUE É SPRING MVC?" do site www.devmedia.com.br.

# 1. Introdução
O Spring MVC é um dos frameworks para desenvolvimento Web mais utilizados hoje em dia. Com ele, temos à nossa disposição uma implementação do padrão MVC em conjunto com os principais recursos do Spring. Aprenda, agora, como esse framework como funciona.

# Apresentando o framework
O Spring MVC é um framework Java criado com o intuito de simplificar e trazer mais produtividade ao desenvolvimento de aplicações web. Com ele, programamos com o padrão arquitetural mais utilizado atualmente, o padrão Model-View-Controller, ao mesmo tempo em que tiramos proveito dos principais recursos do Spring Framework. Com isso, temos como resultado um código organizado, fácil de manter e evoluir.

# O que é MVC?
Mas, o que é MVC? MVC é um padrão criado com o objetivo de organizar a arquitetura da nossa aplicação, a dividindo, basicamente, em três camadas com responsabilidades bem definidas. Por esse motivo dizemos que o MVC é um padrão arquitetural.

Todo sistema desenvolvido sobre esse padrão terá, basicamente, as seguintes camadas:

***Model:*** Local onde devem ser criadas as classes que representam o domínio da aplicação, implementada a persistência de dados, a validação e as regras de negócio;

***View:*** Local onde devemos implementar a interface da nossa aplicação, para que os usuários possam acessar as funcionalidades e consumir seus dados, como uma interface gráfica construída com tecnologias como JSF, HTML, CSS, JavaScript;

***Controller:*** Camada que tem como responsabilidade integrar e intermediar a comunicação entre as camadas Model e View.
A partir disso, quando o usuário clica em um link em uma aplicação web, a View interpretará essa ação e solicitará ao Controller algo que atenda àquele pedido. O Controller, por sua vez, tratará essa requisição e fará a solicitação ao Model. No Model, a lógica é executada, o banco de dados, por exemplo, é consultado, e os dados são repassados ao Controller, que, por fim, envia esses dados para a View, que os exibirá conforme os elementos visuais que ela possui (Figura 1).

![Alt text](https://arquivo.devmedia.com.br/naoexclusivo/EduardoSpinola/OqueeSpringMVC/images/fluxo_execucao_springmvc.png?raw=true "Fluxo de execução de uma aplicação MVC")

***Figura 1. Fluxo de execução de uma aplicação MVC***

Agora, vejamos como o Spring MVC funciona (Figura 2). Ao receber uma requisição (1), o Front Controller, implementado pelo Spring MVC, e que também é chamado de Dispatcher Servlet, irá identificar o controller mais adequado para tratá-la. Então, essa solicitação é passada para ele junto com os dados que possam vir com a requisição (2).

Logo após, o controller identificará o método responsável por atender esse tipo de requisição. Nesse método, o model será chamado (3) para realizar o processamento, por exemplo, obter a lista de carros.

Com essa lista disponível (4), o controller, montará a resposta, um objeto, a ser enviado para o Front Controller (5). Esse objeto é formado, basicamente, por esses dados e mais uma string, que define a página que deve ser exibida.

O Front Controller, então, vai enviar esses dados para a View (6), que identificará a página adequada, incluirá nessa página os dados obtidos pelo model e irá gerar o HTML com esses dados. Feito isso, o View envia essa página ao Front Controller (7) que, por fim, a envia para o browser (8).

![Alt text](https://arquivo.devmedia.com.br/naoexclusivo/EduardoSpinola/OqueeSpringMVC/images/fluxo_springmvc.png?raw=true "Figura 2. Fluxo de funcionamento do Spring MVC")

<strong>Figura 2. Fluxo de funcionamento do Spring MVC</strong>

# 2. Configurando o Spring MVC

Aprenda, agora, a configurar o Spring MVC. Assim como o Spring, este framework pode ser configurado tanto via XML quanto via código Java. Aqui, vamos apresentar como fazer isso a partir da primeira opção: XML.

# Primeiro contato com o Spring MVC
Para ter o primeiro contato com o Spring MVC, vamos analisar o código de uma pequena aplicação web, que tem como simples objetivo listar os carros cadastrados. Essa aplicação é formada basicamente por duas páginas: uma página inicial e uma página responsável pela listagem dos dados.

# Configurando o arquivo pom.xml
Ao começar o projeto, o primeiro passo é configurar as dependências necessárias. Como optamos por um projeto Maven, vamos adicionar essas dependências no pom.xml, apresentado abaixo. Nesse arquivo, começamos declarando a dependência referente ao Spring Framework e, em seguida, declaramos a dependência referente ao Spring MVC.

Logo após, declaramos mais três dependências:

- A API de Servlets, visto que estamos lidando com um projeto web;
- JSTL, pois vamos utilizá-la em nossa página de listagem de carros;
- A API da JavaServer Pages, pois vamos construir nossas páginas com JSP.

Ao final desse arquivo temos ainda as configurações dos plug-ins de compilação e do Tomcat disponibilizados pelo Maven, para compilar nosso código e executar o projeto nesse container web.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
        http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>
  
   <groupId>br.com.devmedia</groupId>
   <artifactId>spring-mvc</artifactId>
   <version>1.0-SNAPSHOT</version>
  
   <packaging>war</packaging>
  
   <dependencies>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>5.0.0.RELEASE</version>
       </dependency>
  
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.0.0.RELEASE</version>
       </dependency>
  
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>javax.servlet-api</artifactId>
           <version>3.1.0</version>
       </dependency>
  
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>jstl</artifactId>
           <version>1.2</version>
       </dependency>
  
       <dependency>
           <groupId>javax.servlet.jsp</groupId>
           <artifactId>javax.servlet.jsp-api</artifactId>
           <version>2.3.1</version>
       </dependency>
   </dependencies>
  
   <build>
       <plugins>
           <plugin>
               <groupId>org.apache.tomcat.maven</groupId>
               <artifactId>tomcat7-maven-plugin</artifactId>
               <version>2.2</version>
               <configuration>
                   <port>8080</port>
                   <path>/spring-mvc</path>
               </configuration>
           </plugin>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-compiler-plugin</artifactId>
               <version>3.3</version>
               <configuration>
                   <source>1.8</source>
                   <target>1.8</target>
               </configuration>
           </plugin>
       </plugins>
   </build>
</project>
```
# Configurando o arquivo web.xml
Por se tratar de um projeto web, precisamos configurar também o web.xml, o qual deve ser criado dentro da pasta webapp/WEB-INF. Nesse arquivo definimos a página inicial da aplicação e, logo após, configuramos o servlet adequado, que no caso do Spring MVC é um Dispatcher Servlet.

```xml
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns="http://xmlns.jcp.org/xml/ns/javaee"
 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
 http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
   id="WebApp_ID" version="3.1">
   <welcome-file-list>
       <welcome-file>index.jsp</welcome-file>
   </welcome-file-list>
   <servlet>
       <servlet-name>Spring MVC Dispatcher Servlet</servlet-name>
       <servlet-class>
           org.springframework.web.servlet.DispatcherServlet
       </servlet-class>
       <init-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>
               /WEB-INF/spring-context.xml
           </param-value>
       </init-param>
       <load-on-startup>1</load-on-startup>
   </servlet>
  
   <servlet-mapping>
       <servlet-name>Spring MVC Dispatcher Servlet</servlet-name>
       <url-pattern>/</url-pattern>
   </servlet-mapping>
</web-app>
```

O Dispatcher Servlet representa o Front Controller e vai abstrair detalhes das requisições HTTP ao mesmo tempo em que saberá encaminhar cada requisição ao controller mais adequado da nossa aplicação.

No web.xml, observe que após a declaração do servlet temos a tag <init-param>, a qual define uma configuração a ser passada para o servlet. Utilizamos essa tag para informar onde estão as configurações do Spring Framework. Dessa forma, quando esse servlet é iniciado, ele buscará pelo arquivo spring-context.xml para que o framework passe a considerar as configurações feitas nesse XML.

Finalizando essa configuração, indicamos que o servlet deve ser carregado ao iniciar a aplicação e fazemos o mapeamento desse servlet.

Configurando o arquivo spring-context.xml
Vejamos agora a configuração do arquivo spring-context.xml, o qual também deve ser criado dentro da pasta webapp/WEB-INF. Nesse arquivo, note que temos uma configuração simples.

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xmlns:mvc="http://www.springframework.org/schema/mvc"
      xsi:schemaLocation="http://www.springframework.org/schema/mvc
   http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd
   http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context-3.0.xsd">
  
   <mvc:annotation-driven />
   <context:component-scan base-package="br.com.devmedia.springmvc" />
  
   <bean class=
    "org.springframework.web.servlet.view.InternalResourceViewResolver">
       <property name="prefix" value="/WEB-INF/views/"/>
       <property name="suffix" value=".jsp"/>
   </bean>
  
</beans>
```
Após o cabeçalho, com a tag <mvc:annotation-driven />, habilitamos o uso de anotações do Spring MVC, e com a tag <context:component-scan>, indicamos o pacote base a partir do qual o Spring deve procurar pelas classes marcadas com anotações do Spring ou do Spring MVC, isto é, onde ele deve procurar pelos beans que ele deve gerenciar.

Por fim, na tag <bean>, configuramos a classe InternalResourceViewResolver. Essa classe representa um recurso interno do framework responsável por localizar o elemento mais indicado para “resolver”, ou melhor, para construir a view a ser apresentada para o usuário. Para isso, note que passamos para ela duas configurações: o local onde devem estar os arquivos que representam a nossa view - neste caso, a pasta /WEB-INF/views/ - e o sufixo desses arquivos - neste caso, .jsp. Por debaixo dos panos, o Spring MVC utilizará esse recurso na camada de visão para obter o arquivo JSP mais adequado.
  
# 3. Spring MVC na prática

Com o framework configurado, analisaremos um projeto exemplo para aprender como é feito o desenvolvimento de uma aplicação com Spring MVC. Nesta aula analisaremos as classes das camadas Model e Controller.

Análise dos fontes
Para começar a análise dos fontes do projeto exemplo, vejamos o código da classe que representa o domínio da nossa aplicação, a classe Carro.

```java
public class Carro {
  
   private String modelo;
   private String marca;
   private int ano;
  
   public Carro() {
   }
  
   public Carro(String modelo, String marca, int ano) {
       this.modelo = modelo;
       this.marca = marca;
       this.ano = ano;
   }
  
   //getters e setters omitidos
  
}
```

Nele, definimos três atributos, representando o modelo, a marca e o ano do carro, e programamos os getters e setters de cada um. Além disso, também podemos notar dois construtores. Aqui, é válido destacar que o construtor padrão é requisitado pelo Spring e pelo Spring MVC. Portanto, caso você crie algum construtor com parâmetros, lembre-se de programar o construtor padrão.

Vejamos agora a classe CarroDao. Como podemos notar, começamos definindo como atributo uma lista de carros. No construtor, inicializamos essa lista, e para isso, chamamos o método privado criarCarros(). Nesse método, por sua vez, simplesmente adicionamos alguns carros à lista. Por fim, temos o método getCarros(), que apenas retorna a lista de carros cadastrados.

```java
@Repository
public class CarroDao {
  
   private static List<Carro> carros;
  
   public CarroDao() {
       criarCarros();
   }
  
   private void criarCarros() {
       if (carros == null) {
           carros = new ArrayList<Carro>();
  
           carros.add(new Carro("Focus", "Ford", 2016));
           carros.add(new Carro("Linea", "Fiat", 2014));
           carros.add(new Carro("Jetta", "Volkswagen", 2015));
           carros.add(new Carro("Cruze", "Chevrolet", 2017));
       }
   }
  
   public List<Carro> getCarros() {
       return carros;
   }
  
}
```
Note que nosso DAO está bastante simples. Optamos por esse caminho para manter o foco no que realmente importa: o aprendizado do Spring MVC. No caso dessa classe, o ponto mais importante pode ser verificado sobre classe. Observe a anotação @Repository. Com ela, informamos ao Spring que essa classe é um bean relacionado à camada de acesso dados e que, a partir de agora, ele deve gerenciar o ciclo de vida desse bean.

Para finalizar nossa análise do código Java, vejamos a classe CarroController. Aqui, saiba que é uma prática adotada pelos programadores adicionar como sufixo no nome da classe o termo Controller. Isso, no entanto, não é obrigatório. Para que o Spring MVC saiba que essa classe é um controller, precisamos anotá-la com @Controller, como apresentado abaixo:

```java
@Controller
@RequestMapping("carros")
public class CarroController {
  
   @Autowired
   private CarroDao dao;
  
   @RequestMapping(value = "/listar", method = RequestMethod.GET)
   public ModelAndView listarCarros(ModelMap model) {
       model.addAttribute("carros", dao.getCarros());
       return new ModelAndView("/carro/list", model);
   }
  
}
```
Em seguida, observe a anotação @RequestMapping. Com ela, determinamos o caminho que deve ser informado na URL para que esse controller seja identificado. Em nosso exemplo, o endereço deve ser: localhost:8080/spring-mvc/carros/.

Dentro do controller, como precisamos nos comunicar com o Model, declaramos um atributo do tipo CarroDao. Lembra que com a anotação @Repository avisamos ao Spring que CarroDao é um repositório? Ao fazer isso, o Spring passará a gerenciar o ciclo de vida desse elemento, o qual também passará a ser visto como um bean. Então, ao anotar o atributo carroDao com @Autowired, o Spring fará a injeção de dependência desse objeto, o deixando pronto para quando precisarmos dele.

Por fim, temos o método listarCarros(). Assim como fizemos com a classe CarroController, anotamos esse método com @RequestMapping. Nessa anotação, informamos o caminho que deve ser acessado para que esse método seja chamado. Neste caso, foi informado o valor “/listar”.

Observe, também, o segundo parâmetro dessa anotação: method = RequestMethod.GET. Dessa forma estamos informando que quando o usuário realizar uma requisição do tipo GET para localhost:8080/spring-mvc/carros/listar, esse é o método que deve ser executado. É utilizando essas informações presentes na URL que o Dispatcher Servlet identificará o controller e o controller saberá qual método deve ser executado.

Agora, repare que esse método retorna um objeto do tipo ModelAndView e recebe como parâmetro um objeto do tipo ModelMap. Ambos são recursos fornecidos pelo Spring e nos permitem abstrair, isto é, encapsular, alguns detalhes sobre como devemos enviar informações para a página a ser exibida como resposta à requisição.

Note que nesse momento já temos o código que realiza o mapeamento de uma requisição a um método no controller. Note, também, que já temos um objeto dao injetado pelo Spring para que nosso controller possa se comunicar com o Model. Agora, precisamos solicitar ao Model que execute o método responsável por nos devolver a lista de carros e preparar o retorno desses dados para a nossa View. É exatamente isso que fazemos nas duas linhas do método listarCarros().











