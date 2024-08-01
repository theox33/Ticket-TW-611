### [🔙 Retour au projet](https://github.com/theox33/Stage-Technique/blob/main/first-nest-app/HOME.md)

## Table des matières

| <div align="left"><h1>🔧 <a href="https://github.com/theox33/Stage-Technique/blob/main/first-nest-app/Controlleurs%20et%20routage.md">Controlleurs et routage</a></h1><ul><li><h2> <a href="https://github.com/theox33/Stage-Technique/blob/main/first-nest-app/Controlleurs%20et%20routage.md#contr&ocirclleurs">:joystick: Controlleurs</a></h2></li><ul><li><h3>📥📤 <a href="https://github.com/theox33/Stage-Technique/blob/main/first-nest-app/Controlleurs%20et%20routage.md#requ%C3%AAtes-get-et-post">Requêtes `@Get` et `@Post`</a></h3></li></ul><li><h2>:compass: <a href="https://github.com/theox33/Stage-Technique/blob/main/first-nest-app/Controlleurs%20et%20routage.md#routage">Routage</a></h2></li> |
|---|

# Controlleurs et routage

## Controlleurs

Les controlleurs sont des modules ou des cmposants chargés de traiter et de répondre aux demandes entrantes des utilisateurs, tels les naviguateurs web ou les consommateurs d'API. Ils sont chargés de traiter les données, de les envoyer à la base de données, de les récupérer et de les renvoyer à l'utilisateur.

Dans le projet que j’ai créé avec NestJS, je possède un controlleur par défaut qui est le fichier `/src/app.controller.ts`.

Un contrôleur dans NestJS est responsable de la gestion des requêtes HTTP entrantes, de leur traitement et de la fourniture d'une réponse appropriée. Les contrôleurs encapsulent la logique pour des routes ou des points d'accès spécifiques de l’application. Ils sont généralement responsables de la validation des données d'entrée, de l'invocation de services ou de la logique métier, et du retour des réponses. Dans NestJS, les contrôleurs sont décorés avec le décorateur @Controller(), et on peut définir les gestionnaires de routes sous forme de méthodes au sein de la classe du contrôleur.

### Requêtes `@Get` et `@Post`

Je créé une nouvelle requête d’entrée `@Get` que je mets sur un point d’entrée différent que la racine : `@Get(‘/askquestion’)`. La requête sera donc accessible vers le lien : `localhost:3000/askquestion`.
Je définie la méthode `askQuestion` qui retourne `’What is your name ?’`.

``` typescript
  @Get('/askquestion')
  askQuestion(): string {
    return 'What is your name?';
  }
```

Ainsi, j’obtiens ceci dans le navigateur :

![image](https://github.com/user-attachments/assets/01b4ce34-7c21-43fb-afd7-277b525baea4)


Maintenant, pour obtenir des données depuis le frontend, il faut utiliser `@Post`. On pourra donc envoyer notre réponse sur le point d’entrée `’/answer’`.
Je créé la méthode `answer()` qui retournera un objet comme réponse.

Il faut savoir que quand on souhaite obtenir les données d’un utilisateur à un moment précis, on doit créer un `DTO` *(Data Transfer Object)*.
Je créé donc un dossier `/src/dto/` où je créé le fichier `app.dto.ts`.

Un `DTO` s’écrit comme un objet classique, on peut utiliser une classe pour cela.
Je définis la classe `AnswerDto` qui aura pour seule valeur : `answer`.

``` typescript
export class AnswerDto {
    answer
}
```

À présent, si je souhaite recevoir des données à partir d’une requête, il faut utiliser `@Body` en paramètre de la méthode.
Je déclare également `getAnswerDto` qui nous fournira les données que l’on souhaite accéder, en l’occurrence : `AnswerDto`.

``` typescript
import { Controller, Get, Post, Body } from '@nestjs/common';
import { AppService } from './app.service';
import { AnswerDto } from './dto/app.dto';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }

  @Get('/askquestion')
  askQuestion(): string {
    return 'What is your name?';
  }

  @Post('/answer')
  answer(@Body() getAnswerDto: AnswerDto) {
    return getAnswerDto.answer
  }
}

```

Finalement, en allant sur mon navigateur web, j’ouvre les outils de développement web et me rends dans la `console réseau`. D’ici, je vais pouvoir envoyer ma requête.

Comme on souhaite obtenir une réponse, je vais envoyer une requête en `JSON` où je vais attribuer une valeur à `answer` :

![image](https://github.com/user-attachments/assets/bd7f2dfb-56bd-4642-8210-fe16ae818fda)


On obtient bien la réponse :

![image](https://github.com/user-attachments/assets/594d6505-4fa8-40ab-b6ca-f2dce0cde2bb)
