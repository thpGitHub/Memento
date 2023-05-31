# NextAuth

````shell
npm install next-auth
````

Création du fichier `[...nextauth].js` dans `pages/api/auth` ainsi toutes les demandes à /api/auth/*( signIn, callback, signOut, etc.) seront automatiquement traitées par NextAuth.js.

Avec l'utilisation de prisma :

````shell
npm install @next-auth/prisma-adapter
````

<https://next-auth.js.org/providers/credentials>

exemple de `[...nextauth].ts`

````typescript
import bcrypt from 'bcrypt'
import NextAuth from 'next-auth/next'
import CredentialsProvider from 'next-auth/providers/credentials'
import { PrismaAdapter } from '@next-auth/prisma-adapter'

import prisma from '@/libs/prismadb'

export default NextAuth({
  adapter: PrismaAdapter(prisma),
    providers: [
        CredentialsProvider({
        name: 'Credentials',
        credentials: {
            email: { label: "Email", type: "text" },
            password: {  label: "Password", type: "password" }
       },
         async authorize(credentials) {
            if(!credentials?.email || !credentials?.password) {
                throw new Error("Invalid credentials")
            }
            // findUnique() method of prisma
            const user = await prisma.user.findUnique({
                where: {
                    email: credentials.email
                }
            })
            if(!user || !user.hashedPassword) {
                throw new Error("Invalid credentials")
            }

            const isCorrectPassword = await bcrypt.compare(credentials.password, user.hashedPassword)

            if(!isCorrectPassword) {
                throw new Error("Invalid credentials")
            }

            return user
        }
       })
    ],
    debug: process.env.NODE_ENV === "development",
    session: {
     strategy: "jwt",   
    },
    jwt: {
        secret: process.env.NEXTAUTH_JWT_SECRET,
    },
    secret: process.env.NEXTAUTH_SECRET,
})
````