# Aula-351.-Named-Query
Aula 351. Named Query --->>> Consulta Nomeada

primeira parte
____________________________________
package teste.consulta;

import java.util.List;
import infra.DAO;
import modelo.muitospramuitos.Filme;

public class ObterFilmes {
	
	public static void main(String[] args) { 
		
		DAO<Filme> dao = new DAO<>(Filme.class);
		List<Filme> filmes = dao.consultar(
			"obterFilmesComNotaMaiorQue", "nota", 8.5); 
	
		System.out.println(filmes.size());
	
		for(Filme filme: filmes) {
			System.out.println(filme.getNome());
		
		}
	}
}
___________________________________
resultado -->
___________________________________

Hibernate: 
    select
        distinct filme0_.id as id1_4_0_,
        ator2_.id as id1_1_1_,
        filme0_.nome as nome2_4_0_,
        filme0_.nota as nota3_4_0_,
        ator2_.nome as nome2_1_1_,
        atores1_.filme_id as filme_id1_2_0__,
        atores1_.ator_id as ator_id2_2_0__ 
    from
        filmes filme0_ 
    inner join
        atores_filmes atores1_ 
            on filme0_.id=atores1_.filme_id 
    inner join
        atores ator2_ 
            on atores1_.ator_id=ator2_.id 
    where
        filme0_.nota>?
1
Star Wars Ep 4
____________________________________
segunda parte
____________________________________

package teste.consulta;

import java.util.List;
import infra.DAO;
import modelo.muitospramuitos.Ator;
import modelo.muitospramuitos.Filme;

public class ObterFilmes {
	
	public static void main(String[] args) {
		
		DAO<Filme> dao = new DAO<>(Filme.class);
		List<Filme> filmes = dao.consultar(
			"obterFilmesComNotaMaiorQue", "nota", 8.5); 
	
		System.out.println(filmes.size());
	
		for(Filme filme: filmes) {
			System.out.println(filme.getNome() +" => " + filme.getNota());
			
			for(Ator ator: filme.getAtores()) {
				System.out.println(ator.getNome());
			}
		}
	}
}
___________________________________
resultado -->
___________________________________
Hibernate: 
    select
        distinct filme0_.id as id1_4_0_,
        ator2_.id as id1_1_1_,
        filme0_.nome as nome2_4_0_,
        filme0_.nota as nota3_4_0_,
        ator2_.nome as nome2_1_1_,
        atores1_.filme_id as filme_id1_2_0__,
        atores1_.ator_id as ator_id2_2_0__ 
    from
        filmes filme0_ 
    inner join
        atores_filmes atores1_ 
            on filme0_.id=atores1_.filme_id 
    inner join
        atores ator2_ 
            on atores1_.ator_id=ator2_.id 
    where
        filme0_.nota>?
1
Star Wars Ep 4 => 8.9
Harrison Ford
Carrie Fisher
___________________________________
fim
<?xml version="1.0" encoding="UTF-8"?>
<entity-mappings version="2.2" xmlns="http://xmlns.jcp.org/xml/ns/persistence/orm" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence/orm http://xmlns.jcp.org/xml/ns/persistence/orm_2_2.xsd">

	<named-query name="obterFilmesComNotaMaiorQue">
	 <query>
	 	select distinct f from Filme f
	 	join fetch f.atores
	 	where f.nota > :nota
	 </query>
	</named-query>

</entity-mappings>
