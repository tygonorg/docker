FROM mcr.microsoft.com/windows/servercore:1803 as installer

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop';$ProgressPreference='silentlyContinue';"]

RUN Invoke-WebRequest -OutFile nodejs.zip -UseBasicParsing "https://nodejs.org/dist/v12.4.0/node-v12.4.0-win-x64.zip"; Expand-Archive nodejs.zip -DestinationPath C:\; Rename-Item "C:\\node-v12.4.0-win-x64" c:\nodejs

FROM mcr.microsoft.com/windows/nanoserver:1803

WORKDIR C:/nodejs
COPY --from=installer C:/nodejs/ .
RUN SETX PATH C:\nodejs
RUN npm config set registry https://registry.npmjs.org/

FROM mcr.microsoft.com/windows/nanoserver:1803

WORKDIR C:/nodejs
COPY --from=installer C:/nodejs/ .
RUN SETX PATH C:\nodejs
RUN npm config set registry https://registry.npmjs.org/

WORKDIR C:/app
ONBUILD COPY src/WebSpa/package.json package.json
# install and cache app dependencies
#COPY src/WebSpa/package.json C:/app/package.json

WORKDIR C:/app/src/WebSpa
RUN npm install
RUN npm install -g @angular/cli@latest

# add app
COPY . /app

# start app
CMD cd /app/src/WebSpa && ng serve --host 0.0.0.0