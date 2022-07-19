# A Simple NestJS API

NestJSで開発したシンプルなAPI(CRUDモデルに従って実装)

# 手順

まずは以下のコマンドを実行する

```
npx nest g resource users
```

## `users/dto/create-users.dto.ts`

```ts
export class CreateUserDto {
    username: string
    email: string
    password: string
}
```

## `users/entities/user.entity.ts`

```ts
export class User {
    id: number
    username: string
    email: string
    password: string
}
```

## `users/users.service.ts`

```ts
import { Injectable } from '@nestjs/common';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';
import { User } from './entities/user.entity';

@Injectable()
export class UsersService {
  private users: User[] = []
  private idSeq = 0
  create(createUserDto: CreateUserDto) {
    this.users.push({
      ...createUserDto,
      id: this.idSeq++
    })
    return this.users.at(-1)
  }

  findAll(): User[] {
    return this.users
  }

  findOne(id: number): User {
    return this.users.find((user) => user.id === id)
  }

  update(id: number, updateUserDto: UpdateUserDto) {
    const i = this.users.findIndex((user) => user.id == id)
    if (i === -1) return null
    this.users[i] = {
      ...this.users[i],
      ...UpdateUserDto
    }
    return this.users[i]
  }

  remove(id: number):User {
    const i = this.users.findIndex((user) => user.id == id)
    if (i === -1) return null
    const user = this.users[i]
    this.users.splice(i, 1)
    return user
  }
}
```


# 開発環境

* Windows 11
* NestJS
* Visual Studio Code 1.68