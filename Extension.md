# Classes d'Extension

Les classes d'extension en C# sont une fonctionnalité puissante qui permet d'étendre les fonctionnalités d'une classe sans modifier directement son code source. Elles offrent une manière élégante d'ajouter de nouvelles méthodes à des types existants, notamment des types primitifs ou des types de bibliothèque.
Elles sont majoritairement utilisées pour écrire de nouvelles bibliothèques basées sur des bibliothèques existantes

## Fonctionnement
---
### 1. Définition de méthodes d'extension

- Les méthodes d'extension sont des méthodes statiques définies dans des classes statiques.
- Le premier paramètre de la méthode doit être du type que vous souhaitez étendre, précédé du mot-clé this.
- Ces méthodes peuvent être appelées comme des méthodes normales sur les instances du type cible.

```csharp
public class Personne
{
	public string Nom { get; set; } = String.Empty;
}

public static class PersonneExtension
{
	public static void DisBonjour(this Personne p)
	{
		Console.WriteLine("Bonjour, je m'appelle " + p.Nom);
	}
}
```
En prenant ces deux classes en exemple, on peut écrire le code suivant :

```csharp
Personne p1 = new() { Nom = "Roger" };

PersonneExtension.DisBonjour(p1);

p1.DisBonjour();
```

Les deux lignes feront appel à la méthode statique de PersonneExtention et afficheront
> Bonjour, je m'appelle Roger

## Avantages
---
- **Extensibilité:** Les classes d'extension permettent d'ajouter de nouvelles fonctionnalités à des types existants sans modifier leur code source, ce qui favorise le principe d'**[ouverture/fermeture]((https://fr.wikipedia.org/wiki/SOLID_(informatique)))**.
- **Lisibilité:** Elles rendent le code plus lisible et maintenable en regroupant des méthodes associées avec le type cible.

## Limitations

- **Accessibilité aux membres privés:** Les méthodes d'extension ne peuvent pas accéder aux membres privés de la classe étendue.

- **Incapacité à remplacer des méthodes existantes:** Les méthodes d'extension ne peuvent pas remplacer les méthodes existantes de la classe cible. En cas d'override, les méthodes de la classe ayant la même signature qu'une méthode de la classe d'extension sera appelée. Si on ajoute à notre class *Personne* la méthode suivante :

```csharp
	public void DisBonjour()
	{
		Console.WriteLine("J'ai la priorité!");
	}
```

Le code suivant :

```csharp
Personne p1 = new() { Nom = "Roger" };

PersonneExtension.DisBonjour(p1);

p1.DisBonjour();
```
affichera cette fois
>Bonjour je m'appelle Roger
>J'ai la priorité!


- **Clarté du code:** L'utilisation excessive de classes d'extension peut rendre le code moins clair, car les méthodes ajoutées peuvent sembler appartenir à la classe cible, il faut trouver le juste milieu.