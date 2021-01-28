## Flow Infrastructure

Developer push code to github then build code into docker images and push it to dorker hub. 

All processes that occur are done automatically using the Jenkins server. 

Then the jenkins server runs the build into kubernetes by fetching the pushed image to dockerhub

## Documentation

The script and command running automatically in jenkinsfile


```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)