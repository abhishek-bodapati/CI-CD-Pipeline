# Uploading Node artifacts to Nexus using Jenkins

### Main idea is to upload the `build/` folder generated after `npm run build` into Nexus Repository
- Nexus repository type - raw(hosted)
- Intially, Jenkins runs the following set of commands
`
npm install && npm run build
`
- A production build folder (`build/`) is generated
- The `build` folder is compressed into `build.zip`
- The compressed `build.zip` is uploaded into Nexus Repository
- Jenkins would also send a HTTP notification after a successful build

 ### Version of the app and Destination IP Address to which notification has to be sent can be configured in `env` file


 ```
 # example usage

VERSION=2.0.0
DESTINATION_SERVER=http://localhost:1080 # sends the notification to this address
 ```