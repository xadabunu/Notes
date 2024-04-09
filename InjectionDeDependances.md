# Inversion  et Injection de Dépendances

L'[Inversion de Dépendance](https://fr.wikipedia.org/wiki/SOLID_(informatique)) et l'Injection de Dépendances sont des concepts fondamentaux en développement logiciel, utilisés pour améliorer la modularité, la maintenabilité des applications. Ils sont souvent utilisés conjointement pour permettre une meilleure gestion des dépendances au sein des applications en automatisant la création d'instantes de dépendances: l'application crée des instances quand elle en a besoin et pour la durée nécessaire (déterminée par le dev dans *Program.cs*).

> ###### Durée de vie des services
> 
> Dans le cas, d'une application web, les Injections de Dépendances sont souvent utilisées pour les instances de services et peuvent avoir 3 durées de vie:
>
> ```csharp
> services.AddTransient<IMyService, MyService>();
> services.AddScoped<IMyService, MyService>();
> services.AddSingleton<IMyService, MyService>();
> // à chaque fois qu'une instance de IMyService est nécessaire,
> // l'application regarde si une instance existe dans le scope correspondant
> ```
> 
> **Transient:** une instance du service est créée à chaque déclaration
> ```csharp
> public void HttpRequest()
> {
> 	MyService s1 = new MyService();
> 	MyService s2 = new MyService();
> 
> 	Console.WriteLine(s1 == s2); //False
> }
> ```
> avantage: threadsafe || désavantage: demande + de mémoire
> 
> **Scoped:** une instance du service est créée à chaque requête
> ```csharp
> public void HttpRequest()
> {
> 	MyService s1 = new MyService();
> 	MyService s2 = new MyService();
> 
> 	Console.WriteLine(s1 == s2); //True
> }
> ```
> avantages: occupe - de mémoire et assure cohérence des données au sein de la requête
> désavantage: les données pourraient être modifiées par une autre requête entre temps
> 
> **Singleton:** une seule instance du service est créée pour toute la durée de vie de l'application
> ```csharp
> public void HttpRequest()
> {
> 	MyService s1 = new MyService();
> }
> public void AnotherHttpRequest()
> {
> 	MyService s2 = new MyService();
> }
> // s1 == s2 => True
> ```
> avantage: demande peu de mémoire
> désavantage: plusieurs appels simultanés au service peuvent créer des interférences
> &nbsp;

## Inversion de Dépendance


L'Inversion de dépendance est un principe de conception dans lequel, au lieu d'une class dépendant d'une implémentation, la classe et l'implémentation dépendent d'abstractions (interfaces). Forçant les deux partis à respecter un contrat.

### Principes clés

- **Dépendance à l'interface:** Les composants dépendent d'abstractions (interfaces) plutôt que d'implémentations concrètes. Cela favorise la flexibilité et l'extensibilité du code.
- **Inversion du contrôle:** La création et la gestion des dépendance est remise au conteneur, à l'application.
- **Modularité accrue:** L'ID favorise la modularité en séparant les préoccupations et en réduisant les couplages entre les différents composants. Seule une modification du contrat entier impliquera une modification profonde du code.

## Injection de Dépendances


L'Injection de Dépendances est une technique utilisée pour fournir les dépendances nécessaires à un composant à partir d'une source externe plutôt que de les instancier directement à l'intérieur du composant. Cela permet de rendre les composants plus flexibles et réutilisables. *(voir services.AddXX plus haut)*

### Types d'Injection de Dépendances

- **Injection de Dépendances par Constructeur:** Les dépendances sont fournies au composant via son constructeur.
- **Injection de Dépendances par Propriété (Setter):** Les dépendances sont injectées via des propriétés publiques ou des méthodes "setter".
- **Injection de Dépendances par Interface:** Les dépendances sont fournies via une interface, permettant un découplage plus fort entre les composants.

## Exemple

```csharp
public class MyService
{
	private MyDep _dependancy;
	public MyService()
	{
		_dependancy = new MyDep();
	}
}

// avec l'injection, le constructeur devient

public MyService(MyDep dependancy)
{
	_dependancy = dependancy
}

// en ajoutant l'utilisation des Primary Constructor, le code devient

public class MyService(MyDep _dependancy) { }
```
> [Primary Constructor](https://www.youtube.com/watch?v=Slvyugn458Q)

## Avantages

- **Découplage des composants:** L'Inversion et l'Injection de Dépendances permettent de réduire les couplages entre les composants, favorisant ainsi la modularité et la maintenabilité du code.
- **Réutilisabilité:** Les composants deviennent plus facilement réutilisables car ils dépendent d'abstractions plutôt que d'implémentations concrètes.

## Limitations

- **Complexité accrue:** La mise en œuvre de l'Inversion et de l'Injection de Dépendances peut ajouter une certaine complexité au code, surtout pour les applications de petite taille.
- **Surcharge cognitive:** Comprendre et maintenir du code utilisant ces principes peut nécessiter une courbe d'apprentissage pour les développeurs moins expérimentés.
