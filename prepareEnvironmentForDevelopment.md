# Website architecture and what development tools we need to operate with Everscale.

## Prepare environment 

### Step 1: Instal Git

```
sudo apt install git
git --version
```

### Step 2: Add SSH key to operate with your Github

```
ssh-keygen -o
```
Enter the password for your SSH key 
Example: 'Test12345'
Then
```
cat ~/.ssh/id_rsa.pub
```
Copy your ssh key and go to your github https://github.com/settings/keys
Pres new SSH key

![Screen Shot 2022-04-26 at 19 34 45](https://user-images.githubusercontent.com/61367249/165353573-d959ee4d-432b-4648-8c60-cb84f2d4afe8.png)

You will be redirected to https://github.com/settings/ssh/new
![Screen Shot 2022-04-26 at 20 00 47](https://user-images.githubusercontent.com/61367249/165353906-3051a17d-f37b-4f43-995e-58ef2edde03e.png)

Enter here your Title
Example: `newkey`
Enter here you ssh key and press Add SSH Key

### Step 2: Instal NVM Manager to run Node JS Apps




