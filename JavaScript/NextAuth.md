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

exemple de `[...nextauth].ts` with a credentials authentication provider

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

- `signIn`

The `signIn` method accepts two arguments: a string identifier for the authentication provider and an object containing the user's credentials.

The first argument, the string identifier, specifies the authentication provider to use. NextAuth.js supports various authentication providers such as Google, Facebook, GitHub, etc. This identifier helps NextAuth.js determine which provider-specific logic to execute during the authentication process.

The second argument is an object containing the user's credentials. This typically includes properties such as email and password, which hold the user's login information.

Exemple `RegisterModal.tsx`

````typescript
import { use, useCallback, useState } from "react"
import axios from "axios"

import useLoginModal from "@/hooks/useLoginModal"
import useRegisterModal from "@/hooks/useRegisterModal"

import Input from "../Input"
import Modal from "../Modal"
import toast from "react-hot-toast"
import {signIn} from "next-auth/react"

const RegisterModal = () => {
  const loginModal = useLoginModal()
  const registerModal = useRegisterModal()
  
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  const [name, setName] = useState('')
  const [username, setUsername] = useState('')
  const [isLoading, setIsLoading] = useState(false)

  const onToggle = useCallback(() => {
    if (isLoading) return
    loginModal.onOpen()
    registerModal.onClose()
  }, [isLoading, loginModal, registerModal])

  const onSubmit = useCallback(async () => {
    try {
        setIsLoading

        await axios.post('/api/register', {
            email,
            password,
            username,
            name
        })

        toast.success('Account created.')

        signIn('credentials', { 
            email,
            password,
        })

        registerModal.onClose()
    } catch (error) {
        console.log(error)  
        toast.error("Something went wrong.")
    } finally {
        setIsLoading(false)
    }
  }, [registerModal, email, password, username, name])
  
  const bodyContent = (
    <div className="flex flex-col gap-4"> 
        <Input 
            type="email"
            value={email}
            disabled={isLoading}
            onChange={(e) => setEmail(e.target.value)}
            placeholder="Email"
        />
        <Input 
            type="text"
            value={name}
            disabled={isLoading}
            onChange={(e) => setName(e.target.value)}
            placeholder="Name"
        />
        <Input 
            type="text"
            value={username}
            disabled={isLoading}
            onChange={(e) => setUsername(e.target.value)}
            placeholder="Username"
        />
        <Input 
            type="password"
            value={password}
            disabled={isLoading}
            onChange={(e) => setPassword(e.target.value)}
            placeholder="Password"
        />
    </div>
  )

    const footerContent = (
      <div className="text-neutral-400 text-center mt-4">
        <p>
          Already have an account?
          <span 
            onClick={onToggle} 
            className="text-white cursor-pointer hover:underline"
          >
            Sign in
          </span>
        </p>
      </div>
    )

  return (
    <div>
        <Modal 
            disabled={isLoading} 
            isOpen={registerModal.isOpen} 
            title="Create an account" 
            actionLabel="Register" 
            onClose={registerModal.onClose}
            onSubmit={onSubmit}
            body={bodyContent}
            footer={footerContent}
            />
    </div>
  )
}

export default RegisterModal
````

- `session`

Dans NextAuth.js, les sessions sont créées après un processus d'authentification réussi. Lorsqu'un utilisateur se connecte ou s'authentifie avec succès à l'aide de NextAuth.js, une session est créée et associée à cet utilisateur.

La création de session se produit en arrière-plan et est généralement gérée par NextAuth.js lui-même. Il gère l'état de la session et fournit des utilitaires pour accéder aux données de session.

Une fois la session créée, NextAuth.js stocke les données de session, telles que l'ID utilisateur ou toute autre information pertinente, dans un magasin de session côté serveur. Les données de session sont généralement cryptées et sécurisées pour empêcher toute falsification ou tout accès non autorisé.

Une fois la session créée, NextAuth.js définit généralement un cookie de session côté client, permettant au navigateur de l'utilisateur d'envoyer le jeton de session avec les demandes suivantes pour authentifier l'utilisateur sur le serveur.

La configuration et le comportement spécifiques de la création de session dans NextAuth.js peuvent être personnalisés en fonction des exigences de votre application. Vous pouvez configurer les options de session, telles que la durée ou l'heure d'expiration, et personnaliser le mécanisme de stockage de session.

Dans l'ensemble, la création de session dans NextAuth.js se produit après une authentification réussie et implique le stockage des données de session côté serveur et la définition d'un cookie de session côté client pour maintenir l'état authentifié pour les demandes ultérieures.
