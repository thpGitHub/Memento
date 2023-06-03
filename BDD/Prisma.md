# Prisma ORM

````shell
npm install prisma --save-dev
````

````shell
npx prisma init
````

Cette commande crée un répertoire prisma avec un fichier `schema.prisma` et un `.env` à la racine du projet.

Exemple d'un fichier `schema.prisma`

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mongodb"
  url      = env("DATABASE_URL")
}

model User {
  id        String   @id @default(auto()) @map("_id") @db.ObjectId
  name      String?
  username  String?   @unique
  bio       String?
  email     String?   @unique
  emailVerified DateTime?
  image     String?
  coverImage String?
  profileImage String?
  hashedPassword String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  followingIds String[] @db.ObjectId
  hasNotification Boolean?

  posts Post[]
  comments Comment[]
  notifications Notification[]
}

model Post {
  id        String   @id @default(auto()) @map("_id") @db.ObjectId
  body      String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  userId    String @db.ObjectId
  likedIds  String[] @db.ObjectId

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
  comments Comment[]
}

model Comment {
  id        String   @id @default(auto()) @map("_id") @db.ObjectId
  body      String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  userId    String @db.ObjectId
  postId    String @db.ObjectId

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
  post Post @relation(fields: [postId], references: [id], onDelete: Cascade)
}

model Notification {
  id        String   @id @default(auto()) @map("_id") @db.ObjectId
  body      String
  createdAt DateTime @default(now())
  userId    String @db.ObjectId

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
}
```

```shell
npx prisma db push
````

```shell
npm install @prisma/client
```

```typescript
// \libs\prismadb.ts

// PrismaClient is the main class provided by Prisma for interacting with the database.
import { PrismaClient } from "@prisma/client"

/**
 * By declaring prisma as a global variable, you can use it throughout your codebase without 
 * needing to import or pass it explicitly to every function or module that requires database access.
 */
declare global {
  // prisma is a type TS can hold an instance of PrismaClient or undefined if it has not been initialized. 
    var prisma: PrismaClient | undefined
}

/**
 * PrismaClient is attached to the `global` object in development to prevent
 * exhausting your database connection limit.
 */ 

const client = global.prisma || new PrismaClient()
if (process.env.NODE_ENV !== "production") global.prisma = client

export default client
```

````typescript
// \pages\api\register.ts
import { NextApiRequest, NextApiResponse } from "next";
import bcrypt from 'bcrypt'
import prisma from '@/libs/prismadb'

export default async function handler(req: NextApiRequest, res: NextApiResponse) {
    if(req.method !== "POST") {
        return res.status(405).end()
    }
    try {
        const { email, username, name, password } = req.body
        const hashedPassword = await bcrypt.hash(password, 12)
        const user = await prisma.user.create({
            data: {
                email,
                username,
                name,
                hashedPassword
            }
        })
        
        return res.status(200).json(user)
    } catch (error) {
        console.error(error)
        return res.status(400).end()
    }
}
````
