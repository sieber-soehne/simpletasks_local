
### Auf Linux installieren

**Debian 12:**
**Curl und git installieren**
```
su -
apt update
apt install -y curl
apt install -y git
```


**Meteor mithilfe von Alternativskript installieren**
https://docs.meteor.com/install

```
curl https://install.meteor.com/ | sh 
```

```
exit
```


**SimpleTasks-Repositorie klonen.**
**Durch das Klonen des SimpleTasks-Repositories wurde das simpletasks Directory erstellt. In das Directory wechseln**
https://docs.github.com/de/repositories/creating-and-managing-repositories/cloning-a-repository
```
git clone https://github.com/fredmaiaarantes/simpletasks.git
cd simpletasks
```

**Installieren Sie die benötigten NPM-Pakete:**
https://github.com/fredmaiaarantes/simpletasks
https://www.youtube.com/watch?v=OWwpyELXiWE&t=78s
```
meteor npm install
```

**Starten Sie die Anwendung:**
Falls eine Fehlermeldung kommt einfach nochmal folgenden Befehl eingeben
![[Pasted image 20240514202022.png]]
```
meteor
```

wenn das nicht funktionert meteor update eingeben.

wenn das auch nicht funktioniert folgenden Befehl eingeben:
```
METEOR_LOG=debug meteor run
```

**Beenden der Anwendung:**
```
Linux: Strg + C
MacOS: control + C
```


Im simpletasks Ordner einen Ordner public im simpletasks Ordner erstellen und dort das Logo reinkopieren  
Überprüfen ob man sich im simple tasks ordner befindet.
```
cd ~/simpletasks
mkdir public
ls
```

Logo in den Public ordner in simpletasks verschieben 
**Pfad überprüfen**
```
mv ~/Downloads/Sieber_und_soehne.jpg ~/simpletasks/public
cd public
ls
```

navbar.jsx öffnen (Logo und farben ändern)
```
cd ~/simpletasks/ui/common/components
nano navbar.jsx
```

In dieser Datei einfach alles rauslöschen und folgenden Code reinkopieren.
src="/Sieber_und_soehne.jpg" statt Sieber_und_soehne.jpg den genauen Dateinamen + Endung des Logos im public Ordner eingeben.
Hier finden auch die Farbanpassungen von weiß auf grau und grau auf schwarz statt.
**control + K bzw. Strg + K** einzelne Zeilen löschen.
```
import React from 'react';
import {
  Box,
  Button,
  Flex,
  Image,
  Stack,
  Text,
  useColorMode,
  useColorModeValue,
} from '@chakra-ui/react';
import { MoonIcon, SunIcon } from '@chakra-ui/icons';
import { Logout } from './logout';

export function Navbar() {
  const { colorMode, toggleColorMode } = useColorMode();

  return (
    <Box>
      <Flex
bg={useColorModeValue('white', 'gray.800')}
        color={useColorModeValue('gray.600', 'white')}
        minH="60px"
        py={{ base: 2 }}
        px={{ base: 4 }}
        borderBottom={1}
        borderStyle="solid"
        borderColor={useColorModeValue('gray.200', 'gray.900')}
        align="center"
      >
        <Flex flex={{ base: 1 }} justify="start">
        <Image
  src="./Sieber_und_soehne.jpg"
  alt="Sieber und Söhne Enterprises"
  boxSize="60px"
  mr="12px"
/>
          <Text
            as="span"
            bgGradient="linear(to-l, #FF1A03, #B8E4F1)"
            bgClip="text"
            fontWeight="bold"
            fontFamily="heading"
            textAlign="left"
          >
            Simple Tasks
          </Text>
        </Flex>

        <Stack
          flex={{ base: 1, md: 0 }}
          justify="flex-end"
          direction="row"
          spacing={6}
        >
          <Button
            onClick={toggleColorMode}
            aria-label={colorMode === 'light' ? 'Moon Icon' : 'Sun Icon'}
          >
            {colorMode === 'light' ? <MoonIcon /> : <SunIcon />}
          </Button>
          <Logout />
        </Stack>
      </Flex>
    </Box>
  );
}
```


https://forums.meteor.com/t/unable-to-display-images/40095/5


Dann wieder in den simpletasks Ordner und schauen ob es funktioniert.
```
cd ~/simpletasks
```

```
meteor
```


```
cd ~/simpletasks/ui/pages/tasks
nano tasks-page.jsx
```


Wie vorher alles rauslöschen und folgendes reinkopieren. (**Navbar ändern**)

**Vlt hier für das restliche UI die farben reinhauen**

```
import React, { Suspense } from 'react';
import { TaskForm } from './components/task-form';
import {
  Box,
  Button,
  Heading,
  HStack,
  Spinner,
  Stack,
  Text,
  useColorModeValue,
} from '@chakra-ui/react';
import { TaskItem } from './components/task-item';
import { useTasks } from './hooks/use-tasks';

/* eslint-disable import/no-default-export */
export default function TasksPage() {
  const { hideDone, setHideDone, tasks, count, pendingCount } = useTasks();
  return (
    <>
      <Stack textAlign="center" spacing={{ base: 8 }} py={{ base: 10 }}>
        <Heading fontWeight={600}>
          <Text
            as="span"
            bgGradient="linear(to-l, #FF1A03, #B8E4F1)"
            bgClip="text"
          >
            Simple Tasks
          </Text>
        </Heading>
      </Stack>
      <TaskForm />
      <Suspense fallback={<Spinner />}>
        <Box
          mt={8}
          py={{ base: 2 }}
          px={{ base: 4 }}
          pb={{ base: 4 }}
          border={1}
          borderStyle="solid"
          borderRadius="md"
          borderColor={useColorModeValue('gray.200', 'gray.700')}
        >
          <HStack mt={2}>
            <Box w="70%">
              <Text
                as="span"
                color={useColorModeValue('gray.600', 'gray.400')}
                fontSize="xs"
              >
                You have {count} {count === 1 ? 'task ' : 'tasks '}
                and {pendingCount || 0} pending.
              </Text>
            </Box>
            <Stack w="30%" justify="flex-end" direction="row">
              <Button
                bg="teal.600"
                color="white"
                colorScheme="teal"
                size="xs"
                onClick={() => setHideDone(!hideDone)}
              >
                {hideDone ? 'Show All Tasks' : 'Show Pending'}
              </Button>
            </Stack>
          </HStack>
          {tasks.map(task => (
            <TaskItem key={task._id} task={task} />
          ))}
        </Box>
      </Suspense>
    </>
  );
}
```

```
nano ~/simpletasks/ui/pages/auth/sign-in-page.jsx
```

```
import React from 'react';
import {
  Box,
  Button,
  Flex,
  FormControl,
  FormErrorMessage,
  Heading,
  Input,
  InputGroup,
  InputRightElement,
  Stack,
  Text,
  useColorModeValue,
} from '@chakra-ui/react';
import { useUserId } from 'meteor/react-meteor-accounts';
import { useLogin } from './hooks/use-login';
import { Navigate } from 'react-router-dom';
import { routes } from '../../routes';

/* eslint-disable import/no-default-export */
export default function SignInPage() {
  const userId = useUserId();
  const {
    loginOrCreateUser,
    isSignup,
    setIsSignup,
    showPassword,
    setShowPassword,
    register,
    formState: { errors, isSubmitting },
    handleSubmit,
  } = useLogin();

  if (userId) {
    return <Navigate to={routes.tasks} />;
  }

  return (
    <Flex align="center" justify="center">
      <Stack spacing={8} mx="auto" maxW="lg" py={12} px={6}>
        <Stack align="center">
          <Heading
            fontSize="4xl"
            bgGradient="linear(to-l, #FF1A03, #B8E4F1)"
            bgClip="text"
          >
            Sign in to your account
          </Heading>
          <Text fontSize="lg" color={useColorModeValue('gray.600', 'gray.400')}>
            to start creating your simple tasks
          </Text>
        </Stack>
        <Box
          rounded="lg"
          bg={useColorModeValue('white', 'gray.700')}
          boxShadow="lg"
          p={8}
        >
          <form onSubmit={handleSubmit(loginOrCreateUser)}>
            <Stack spacing={4}>
              <FormControl isInvalid={!!errors.username}>
                <Input
                  id="username"
                  placeholder="Enter your username"
                  autoComplete="username"
                  {...register('username')}
                />
                <FormErrorMessage>{errors.username?.message}</FormErrorMessage>
              </FormControl>
              <FormControl isInvalid={!!errors.password}>
                <InputGroup size="md">
                  <Input
                    id="password"
                    type={showPassword ? 'text' : 'password'}
                    placeholder="Enter your password"
                    autoComplete="current-password"
                    {...register('password')}
                  />
                  <InputRightElement width="4.5rem">
                    <Button
                      h="1.75rem"
                      size="sm"
                      onClick={() => setShowPassword(!showPassword)}
                    >
                      {showPassword ? 'Hide' : 'Show'}
                    </Button>
                  </InputRightElement>
                </InputGroup>
                <FormErrorMessage>{errors.password?.message}</FormErrorMessage>
              </FormControl>
              {!isSignup && (
                <>
                  <Stack spacing={10}>
                    <Button
                      type="submit"
                      bg="blue.600"
                      color="white"
                      _hover={{
                        bg: 'blue.500',
                      }}
                      isLoading={isSubmitting}
                    >
                      Sign in
                    </Button>
                  </Stack>
                  <Stack spacing={10}>
                    <Button onClick={() => setIsSignup(true)}>
                      Create a new account
                    </Button>
                  </Stack>
                </>
              )}

              {isSignup && (
                <>
                  <Stack spacing={10}>
                    <Button
                      type="submit"
                      bg="green.600"
                      color="white"
                      _hover={{
                        bg: 'green.500',
                      }}
                      isLoading={isSubmitting}
                    >
                      Sign up
                    </Button>
                  </Stack>
                  <Stack spacing={10}>
                    <Button onClick={() => setIsSignup(false)}>
                      I have an account
                    </Button>
                  </Stack>
                </>
              )}
            </Stack>
          </form>
        </Box>
      </Stack>
    </Flex>
  );
}
```


Button add task farbe ändern:
```
nano ui/pages/tasks/components/task-form.jsx
```

```
import React from 'react';
import { Box, Button, FormControl, FormErrorMessage, Input, InputGroup, InputRightElement } from '@chakra-ui/react';
import { useTaskForm } from '../hooks/use-task-form';

export function TaskForm() {
  const { saveTask, handleSubmit, register, formState: { errors, isSubmitting } } = useTaskForm();

  return (
    <Box>
      <form onSubmit={handleSubmit(saveTask)}>
        <InputGroup size="md">
          <FormControl isInvalid={!!errors.description}>
            <Input
              h="2.6rem"
              pr="6rem"
              id="description"
              {...register('description')}
              placeholder="Type to add new tasks"
            />
            <FormErrorMessage>{errors.description?.message}</FormErrorMessage>
          </FormControl>
          <InputRightElement width="6rem">
            <Button
              h="2.5rem"
              size="sm"
              bgGradient="linear(to-l, #FF1A03, #B8E4F1)"
              color="white"
              type="submit"
              isLoading={isSubmitting}
              colorScheme="blue"
            >
              Add Task
            </Button>
          </InputRightElement>
        </InputGroup>
      </form>
    </Box>
  );
}
```

**Nach den Änderungen sollte die Applikation folgendermaßen aussehen:**
![[Pasted image 20240515145521.png]]
![[Pasted image 20240515144654.png]]

# Meteor UP
```
su -
apt install npm
exit

npm install -g mup
```

### Neue AWS Instance erstellen
https://github.com/fknipp/biti-weba-cheatsheets/blob/main/creating-ec2-instance.md

Den Port 3000 freigeben
```
#In der neuen AWS Instance folgende Befehle eingeben
sudo yum update
sudo yum install -y docker
#sudo usermod -a -G docker ec2-user
docker --version
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
sudo docker run hello-world
```

# MongoDB Datenbank erstellen
https://www.mongodb.com/products/platform/atlas-database
Dort account erstellen
alles ausfüllen
Frankfurt passt
den button links neben finsh drücken
Dort shared auswählen
dann create cluster
create user
dann auf cloud environment wechseln
finish and close 
dann auf network access
add ip address
allow access from anywhere anklicken
confirm
databse access 
edit
built in role 
dort auf atlas admin setzen
update user
database -> connect -> drivers -> connection string kopieren

diese in der mup.js 
bei mongo url einfügen dann passwort auf das festgelegte passwort ändern

```
#Meteor UP install
su -
apt install npm
npm install -g mup
#Deploy
mup init
cd. deploy 
ls
nano mup.js
mup setup
mup deploy
#für neue config reinhauen bzw jo
mup reconfig
```
### Zusatzaufgabe
**Installation in Meteor Cloud**

- Personal Access Token erstellen
	https://docs.github.com/de/enterprise-server@3.9/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token

- **Github Repository erstellen**
	https://docs.github.com/de/repositories/creating-and-managing-repositories/quickstart-for-repositories

**Projekt auf Github veröffentlichen**

```bash
# Navigieren Sie zum Projektordner
cd ~/simpletasks

# Initialisieren Sie das Git-Repository
git init

# Fügen Sie das Remote-Repository hinzu
git remote add origin https://github.com/sieber-soehne/simpletasks.git

# Fügen Sie alle Dateien hinzu
git add .

# Commiten Sie die Dateien
git commit -m "Initial commit"

# Pushen Sie den Branch 'main' zu GitHub
git push -u origin main

# Beispiel für Eingabeaufforderung
Username for 'https://github.com': USERNAME
Password for 'https://Memo-HD@github.com': YOUR_PERSONAL_ACCESS_TOKEN
```


Falls folgendes kommt einfach den folgenden Schritten folgen:
Git Repository löschen (Einstellungen im Repository -> ganz runterscrollen und löschen)und ein neues erstellen:
```
git push -u origin main
Username for 'https://github.com': sieber-soehne
Password for 'https://sieber-soehne@github.com': 
remote: Permission to fredmaiaarantes/simpletasks.git denied to sieber-soehne.
fatal: unable to access 'https://github.com/fredmaiaarantes/simpletasks.git/': The requested URL returned error: 403

debian@debian:~/simpletasks$ git init
Reinitialized existing Git repository in /home/debian/simpletasks/.git/
debian@debian:~/simpletasks$ git remote add origin https://github.com/sieber-soehne/simpletasks.git
error: remote origin already exists.
debian@debian:~/simpletasks$ git remote -v
origin	https://github.com/fredmaiaarantes/simpletasks.git (fetch)
origin	https://github.com/fredmaiaarantes/simpletasks.git (push)
debian@debian:~/simpletasks$ git remote set-url origin https://github.com/sieber-soehne/simpletasks.git
debian@debian:~/simpletasks$ git push -u origin main

```

Deployen dauert nur eine halbe ewigkeit circa 10min
**Tutorial Installation in Meteor Cloud:** 
	https://www.youtube.com/watch?v=VsyHIRel9ZE&t=237s

**Link zu Meteor Cloud**
	https://cloud.meteor.com/

Nach dem deployen hat circa 14min gedauert läuft die Applikation in der meteorcloud
**Meine Applikation ist erreichbar unter:**
https://kilian.sieber.simpletasks.meteor.meteorapp.com
![[Pasted image 20240515152955.png]]

![[Pasted image 20240515153017.png]]

![[Pasted image 20240515153110.png]]

**Installation mit Docker**
