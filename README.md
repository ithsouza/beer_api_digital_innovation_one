# API de Gerenciamento de Estoque de Cerveja – Testes Unitários

## Descrição do Projeto

Este projeto demonstra o desenvolvimento de **testes unitários para uma API REST** de gerenciamento de estoques de cerveja usando **Spring Boot**, **JUnit 5** e **Mockito**.
Durante o projeto, aplicamos também o **TDD (Test-Driven Development)** em funcionalidades de incremento e decremento de estoque. Ele serve como referência prática para aprendizado e portfólio de desenvolvedores.

## Objetivos

* Compreender conceitos de testes e a **pirâmide de testes**.
* Desenvolver **testes unitários** para funcionalidades básicas da API:

  * Criação de cerveja;
  * Listagem de cervejas;
  * Consulta por nome;
  * Exclusão de cerveja.
* Aplicar **TDD** em funcionalidades importantes:

  * Incremento de estoque;
  * Decremento de estoque.
* Praticar frameworks de testes unitários em Java: **JUnit**, **Mockito** e **Hamcrest**.

## Pré-requisitos

* **Java 14** ou superior
* **Maven 3.6.3** ou superior
* IDE de sua preferência (IntelliJ IDEA Community Edition recomendado)
* Controle de versão **Git** instalado

## Como Executar

### Rodar a aplicação

```bash
mvn spring-boot:run
```

Acesse a API no navegador ou Postman:

```
http://localhost:8080/api/v1/beers
```

### Executar os testes

```bash
mvn clean test
```

## Exemplo de Testes Unitários

```java
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

public class BeerServiceTest {

    @Mock
    private BeerRepository beerRepository;

    @InjectMocks
    private BeerService beerService;

    private Beer beer1;
    private Beer beer2;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);

        beer1 = new Beer(1L, "Heineken", 10);
        beer2 = new Beer(2L, "Budweiser", 15);
    }

    @Test
    void testCreateBeer() {
        when(beerRepository.save(beer1)).thenReturn(beer1);

        Beer created = beerService.createBeer(beer1);
        assertNotNull(created);
        assertEquals("Heineken", created.getName());
        verify(beerRepository, times(1)).save(beer1);
    }

    @Test
    void testListBeers() {
        when(beerRepository.findAll()).thenReturn(Arrays.asList(beer1, beer2));

        List<Beer> beers = beerService.listBeers();
        assertEquals(2, beers.size());
        assertTrue(beers.contains(beer1));
        assertTrue(beers.contains(beer2));
    }

    @Test
    void testGetBeerByName() {
        when(beerRepository.findByName("Heineken")).thenReturn(Optional.of(beer1));

        Beer found = beerService.getBeerByName("Heineken");
        assertNotNull(found);
        assertEquals("Heineken", found.getName());
    }

    @Test
    void testDeleteBeerById() {
        doNothing().when(beerRepository).deleteById(1L);

        beerService.deleteBeerById(1L);
        verify(beerRepository, times(1)).deleteById(1L);
    }
}
```

## Links Úteis

* [Spring Boot](https://spring.io/projects/spring-boot)
* [JUnit 5](https://junit.org/junit5/)
* [Mockito](https://site.mockito.org/)
* [Hamcrest](http://hamcrest.org/)
* [Pirâmide de Testes – Martin Fowler](https://martinfowler.com/articles/practical-test-pyramid.html)
* [Referências de testes com Spring Boot](https://spring.io/guides/gs/testing-web/)
* [Padrão arquitetônico REST](https://restfulapi.net/)
