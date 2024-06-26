# Repository Pattern

Le Repository Pattern est un design pattern séparant la responsabilité de l'accès aux données (donc la connexion à la base de données ainsi que toute interaction avec elle) des classes du model utilisant un Repository.

## Fonctionnement

### 1. Abstraction de l'accès aux données

- Les repositories fournissent une abstraction (interface IRepository) pour l'accès aux données en exposant des méthodes pour effectuer des opérations CRUD (Create, Read, Update, Delete) sur les entités.

```csharp
public interface IRepository<T>
{
    IEnumerable<T> GetAll();
    T GetById(int id);
    void Add(T entity);
    void Update(T entity);
    void Delete(T entity);
}
```

- Ils encapsulent la logique d'accès aux données et masquent les détails de mise en œuvre spécifiques. Tout ce qui concerne la connexion à la base de données, la lecture des données, leur mapping se retrouvent dans le Repository.

```csharp
public class CustomerRepository : IRepository<Customer>
{
    // Implémentation des méthodes CRUD
}
```

### 2. Séparation des préoccupations

- Le Repository Pattern sépare les responsabilités en isolant la logique métier de l'accès aux données.
- Cela permet une meilleure organisation du code et facilite la maintenance et l'évolutivité du système.

### 3. Réusabilité

- Les repositories sont conçus pour être réutilisables et peuvent être utilisés par différentes parties de l'application sans avoir besoin de dupliquer la logique d'accès aux données.

## Avantages 

- **Séparation des préoccupations:** Le pattern Repository sépare clairement la logique métier de l'accès aux données, facilitant ainsi la maintenance et le test du code.
- **Abstraction de la source de données:** Les détails spécifiques de la source de données sont masqués derrière une interface commune, ce qui rend le code plus flexible et moins dépendant des implémentations spécifiques.

## Limitations

- **Complexité accrue:** L'ajout d'une couche de repositories peut augmenter la complexité globale de l'application, surtout pour les applications de petite taille.

# Dapper

Librairie qui se construit autour de ADO.NET pour apporter de nouvelles méthodes simplifiant l'accès et l'interaction avec les données.<br>
Elle est comparable à Entity Framework mais nous laisse maître du code SQL généré là où EF produit son propre code à travers les requêtes LINQ.
Elle ajoute des méthodes d'extension à la classe SqlConnection permettant d'alléger notre code :

```csharp
var connectionString = "...";
using SqlConnection connection = new(connectionString);

var query = "...";

// le code suivant

using SqlCommand command = new(query, connection);
using SqlDataReader reader = command.ExecuteReader();

while (reader.Read())
{
    // handle the data
}

// devient

data = connection.Query(query); //data est un IEnumerable<dynamic>
foreach(var var_name in data)
{
    // handle the data
}

// ou bien en précisant le type

data = connection.Query<Demande>(query); //data est un IEnumerable<Demande>
foreach(var var_name in data)
{
    // handle the data
}
```

Il y a les méthodes `Query` pour les get, `Execute` pour les autres ou encore `Single/OrDefault/Async` etc
