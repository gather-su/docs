## Permissions

## Dictionaries
* EntityType
* Details
* Link
* Avatar
* Contact

## Models

<details>
<summary><code>Entity</code></summary>

* `id` - quid;
* `displayName` - string;
* `alias` - string;
* `type` - quid [`EntityType` external dictionary](#dictionaries);
* `entityDetails` - quid [`Details` external dictionary](#dictionaries);
* `is_deleted` - boolean;


```ts
type Message = {
    entityId: string;
    alias: string;
    typeId: string; // quid
    entityDetails: string;
    isDeleted: boolean;
};
```

</details>

# API

###  Entity - сущность
```
Сущность платформы включает в себя:
- пользователя
- сообщество
- мероприятие
Тип сущности опредляется на этапе создания сущности.
```

<details>
<summary><code>POST /entity/</code></summary>

#### Description

Создание сущности


#### Request body:

* `displayName` - required; max 64;
* `alias` - optional, max 32;


```json
{
  "displayName": {
    "type": "string",
    "required": true,
    "maxLength": 64
  },
  "alias": {
    "type": "string",
    "optional": true,
    "maxLength": 32
  }
}
```

#### Validations:

* В случае ошибки валидации по типу или по значению - ошибка `400`;
* В случае если `alias` уже присутствует в системе `409`;


#### Responses:

* `200`:
    * Response body:
      [`Entity`](#models)
* `400`:
    * Response body:
      [`ValidationError`](#models)
* `409`:
    * Response body:
      `Conflict`
    * Massege: 
        `К сожалению, данный alias уже занят, попробуйте воспользоваться другим`
    * ```json 
      {
      "error": "Conflict",
      "message": "К сожалению, данный alias уже занят, попробуйте воспользоваться другим"
      }
    ```

</details>


<details>
<summary><code>GET /entity/{entityId}</code></summary>

#### Description

Получить сущность по идентификатору 


#### Request body:

* `entityId` - идентификатор пользователя;

```json
{
  "entityId": {
    "type": "string",
    "required": true,
    "maxLength": 255
  }
```

#### Validations:


#### Responses:

* `200`:
    * Response body:
      [`Entity`](#models)
    * ```json
        {
        "id": "1234567890",
        "displayName": "Иван Иванов",
        "alias": "ivan_ivanov"
        }
        ```

* `500`:
    * Response body:
      [`Internal Server Error`](#models)

</details>

<details>
<summary><code>GET /entities?type</code></summary>

#### Description

Вывод всех сущностей по типу

#### Request body:

* `type` - идентификатор типа сущности

```json
{
  "type": {
    "type": "string",
    "required": true,
    "maxLength": 255
  }
```

#### Validations:



#### Responses:

* `200`:
    * Response body:
      [`Entity`](#models)
    * ```json
        {
        "id": "1234567",
        "displayName": "Tech Meetup",
        "alias": "TechMeetup"
        },
      {
        "id": "1235367",
        "displayName": "BeerJS",
        "alias": "beerjs"
        }
        ```
* `404`:
    * Response body:
      [`Not FoundError`](#models)

* `500`:
    * Response body:
      [`Internal Server Error`](#models)

</details>

<details>
<summary><code>GET /currentEntity/{entityId}</code></summary>

#### Description

Получить сущность принадлежащую пользователю


#### Request body:

* `entityId` - идентификатор текущего пользователя;

```json
{
  "entityId": {
    "type": "string",
    "required": true,
    "maxLength": 255
  }
```

#### Validations:

* В случае если `alias` уже присутвует в системе `409`;


#### Responses:

* `200`:
    * Response body:
      [`Entity`](#models)
    * ```json
        {
        "id": "1234567",
        "displayName": "Tech Meetup",
        "alias": "TechMeetup"
        }
        ```
* `404`:
    * Response body:
      [`Not FoundError`](#models)

* `500`:
    * Response body:
      [`Internal Server Error`](#models)

</details>