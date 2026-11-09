# metaltura/node-react

La pile Node + Express + React : les patrons qui écrivent, depuis un modèle de domaine, une application avec un serveur Node et une interface React, puis son code. Un modèle qui branche ces patrons sur un domaine obtient un projet npm qui démarre par `npm install` puis `npm start`.

## Ce qu'il contient

- `models/DomainToNodeReact.yaml` — les patrons de la pile, du domaine vers l'application, en trois paquets :
  - `mongo` : les collections, leurs champs et leurs références ;
  - `node` : les modèles Mongoose, les services, les routeurs, les deux instances de `StackApplication` de la pile (`nodeApplication` et `nodeAuditedApplication`) et le patron de tête `domainToNode`. Il produit trois racines dans la cible de son Mapping : l'Application, le paquet `node` avec le serveur Express et le paquet `react` avec l'interface React ;
  - `react` : le client d'API React.
- `models/NodeReactToCode.yaml` — les patrons de génération de la pile, en deux paquets :
  - `node` : `NodePattern`, la base des patrons qui lisent un routeur ou un service Node (ce que sert un routeur, son modèle, le nom d'une fonction de service), puis le projet TypeScript (`package.json` · `tsconfig.json` · `src/index.ts`, qui répond 400 avec la raison de chaque champ en erreur quand Mongoose refuse un enregistrement), un modèle Mongoose, un service et un routeur par élément du serveur, les types des charges utiles, et le patron de tête `nodeToCode`.
  - `react` : le projet Vite + React + TypeScript d'une interface React (`package.json` · `tsconfig.json` · `vite.config.ts` qui renvoie le chemin de base de l'API vers l'adresse du serveur · `index.html` · `src/main.tsx` · `src/App.tsx` avec une route par route de l'interface), le cadre (la barre de navigation avec le titre et la couleur de la Navbar · un lien par entrée de menu), un fichier par composant de page (un tableau, une fiche avec son bouton « Delete » ou un formulaire avec un contrôle par champ · la liste déroulante d'un champ de relation remplie par la ressource qu'il nomme), un client d'API par service (une fonction par point d'entrée servi), les types des charges utiles par `resourceToPayloadTypes` de `metaltura/application`, et le patron de tête `reactToCode` (option `apiUrl` : l'adresse du serveur · `http://localhost:3000` par défaut). Une page dont le client d'API ne sert pas l'opération qu'elle demande affiche « Not available ». Le patron de tête de pile `nodeReactToCode` part d'un serveur Node et écrit un seul projet : une racine npm workspaces (`package.json` · `README.md` · `.gitignore`), le serveur sous `server/` par `nodeToCode` et le client de l'interface React de la même application sous `client/` par `reactToCode` ; `npm install` puis `npm start` à la racine démarrent les deux.

`metaltura/examples` montre la pile branchée sur le domaine d'une boutique (`BoutiqueNodeReact`).

## Ce dont il dépend

| Dépôt | Ce qu'il en emploie | Ligne `depends` |
| --- | --- | --- |
| `metaltura/domain` | `Domain`, que lit `DomainToNodeReact` | `metaltura/domain/Domain@v1.0.0` |
| `metaltura/application` | `Application`, `DomainToApplication` et `ApplicationToCode` | `metaltura/application/Application@v1.0.0`<br>`metaltura/application/DomainToApplication@v1.0.0`<br>`metaltura/application/ApplicationToCode@v1.0.0` |
| `metaltura/node` | `Node` | `metaltura/node/Node@v1.0.0` |
| `metaltura/react` | `React` | `metaltura/react/React@v1.0.0` |

Ces modèles système de Metaltura s'écrivent par leur nom seul :

- `Mapping` ;
- `FileSystem` ;
- `Diagram` ;
- `Uml`.

## L'utiliser depuis un autre dépôt

Un modèle qui branche la pile sur son domaine écrit dans sa liste `depends` les patrons qu'il emploie, avec les métamodèles que la cible de ses Mappings contient :

```yaml
depends:
  - metaltura/application/Application@v1.0.0
  - metaltura/node/Node@v1.0.0
  - metaltura/react/React@v1.0.0
  - metaltura/application/DomainToApplication@v1.0.0
  - metaltura/node-react/DomainToNodeReact@v1.0.0
  - metaltura/node-react/NodeReactToCode@v1.0.0
```

C'est la liste de `BoutiqueNodeReact` dans `metaltura/examples`. Elle nomme en plus, par leur nom seul :

- `Mapping` ;
- son domaine, `BoutiqueDomain` ;
- `FileSystem`.

Metaltura clone chaque dépôt nommé à l'étiquette `v1.0.0`. Toute la librairie est à `v1.0.0` : Metaltura refuse qu'un même dépôt soit demandé à deux versions.

## D'où il vient

Ce dépôt vient du dépôt privé `Kogitec/simple-mda`, `main` à `96a1a75` (PLAT-1461), rangé par sujet par PLAT-1498 en un seul commit : ses modèles étaient sous `models/applications/node-react/`, avec `Node` et `React`. L'historique de ses fichiers reste dans `simple-mda`, archivé.
