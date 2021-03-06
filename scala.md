Traits vs classes vs interfaces

Trait hacen lo mismo que clases en Scala: pueden tener implementacion y estado interno. Esto las distingue de las interfaces de java.

Limitaciones:
- Un trato no puede tener class parameters (parametros de constructor)
- Si llamamos a super, solo sabemos en tiempo de ejecucion cual es la implementacion que estamos llamando.

Traits == Interfaces in java8

case class:
It is the way to allow pattern matching. It avoids boilerplate. It is a way to ADD more syntax (functionality) to our classes:
- Adds factory method: no need to use new
- All parameters the case class receives (params for the constructor) are stored as a val class attributes
- Adds implementation for: toString, hashCode, equals, copy

Sealed class:
Are classes that can not be instantiated. They are used in case classes to avoid pattern matching of subclasses that you dont want, cause it could be a security problem.

Type class:

Companion Object:
It is an object which goes with a class to store ALL the STATIC things it needs. In the class we just have instance functions and attributes, while in the object we have class functions and attributes (shared by all the instances)

Abstract class (or trait)
As Java, is a class which is not completely defined == has abstract methods (methods without implementation).

Facts:
- Everytime you construct an object with a method (ex: List(2)), you are using a factory. It allows us to avoid using new.
- partial function is a function which is not defined for all the domain of its params.\
- a class is a blueprint of an object
- in Scala, the == operator calls equals(a:Any) method.


Notas about patternmatching en listas:
- :: es el operador cons, SOLO DISPONIBLE CUANDO SE HACE PATTERN MATCHING CON LISTAS
- El orden importa, va evaluando de primero a ultimo.
- En el caso sencillo, tengo que poner x :: Nil porque sino traga todas las expresiones.
- el nombre que le de a cada cosa es irrelevante. Si pongo x :: xs :: y => entonces x es el primer elemento, xs el segundo y y el resto.

LINKS:
http://bkpathak.github.io/scala-substitution-model Explanation of scala substitution model
http://bigocheatsheet.com/ Very good cheat sheet about big O
