# How to Build and Deploy a Full-Stack React-App
Created : 2022-04-17 20:22


- We use [Infrastructure-Components](https://www.infrastructure-components.com/page?ref=medium_fullstack&dest=/). These React Components let us [define our infrastructure architecture as part of our React-app](https://www.infrastructure-components.com/page?ref=medium_fullstack&dest=https://medium.com/dailyjs/do-you-like-react-then-why-dont-you-configure-your-infrastructure-with-it-e8cb36e742a2). We don’t need any other configuration like Webpack, Babel, or Serverless anymore.
- Set up your project in three ways:
	- Download your customized boilerplate code from [www.infrastructure-components.com](https://www.infrastructure-components.com/page?ref=medium_fullstack&dest=/),
	- or clone this [GitHub-repository](https://www.infrastructure-components.com/page?ref=medium_fullstack&dest=https://github.com/infrastructure-components/serviceoriented_example),
	- or install the libraries manually, see [this post](https://www.infrastructure-components.com/page?ref=medium_fullstack&dest=https://codeburst.io/how-to-create-a-service-oriented-react-web-app-2770f41a43d8) for more details.
	- once you installed the dependencies (run `npm install`), you can build the project with a single command: `npm run build`
- The build script adds three more scripts to the `package.json`. They are ready to use!
	-  `npm run {your-project-name}` starts your React-app locally (hot-dev mode, without backend and database)
	-  `npm run start-{your-env-name}` starts the whole software stack locally.  
    _Note_: Starting the database in offline mode requires the Java 8 JDK. [Here’s](https://www.infrastructure-components.com/page?ref=medium_fullstack&dest=https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html) how you can install the JDK.
    -  `npm run deploy-{your-env-name}` deploys your app to AWS.  
    _Note_: In order to deploy your app to AWS, you need a technical IAM user with [these rights](https://www.infrastructure-components.com/page?ref=medium_fullstack&dest=https://gist.github.com/frankzickert/3ef5586a795c5c4a0690a89ae948daf4). Put the user’s credentials into your `.env`-file like this:  
    `AWS_ACCESS_KEY_ID=***   AWS_SECRET_ACCESS_KEY=***`
- Define the Architecture of Your App
	- [Infrastructure-Components](https://www.infrastructure-components.com/page?ref=medium_fullstack&dest=/)-based projects have a clear structure. You have a single top-level component. This defines the overall architecture of your app. Sub-components (children) refine the app’s behavior and add functions.
	- example: 
		-  In the following example, the `<ServiceOrientedApp/>`-component is our top-level component. We export it as default in our entry point file (`src/index.tsx`).
		 ```js
		 export default (
			 <ServiceOrientedApp
			 stackName = "soa-dl"
			 buildPath = 'build'
			 region='eu-west-1'>
				 <Route
					 path='/'
					 name='My Service-Oriented React App'
					 render={()=><DataForm />}
				/>
			 <DataLayer id="datalayer">
				<UserEntry />
				<GetUserService />
				<AddUserService />
			</DataLayer> 
			 </ServiceOrientedApp>
		 );
		 
		 
			```
		-  A `<ServiceOrientedApp/>` is an interactive web-application. You can refine the behavior of this app through the child-components you provide. It supports `<Environment/>`, `<Route/>`, `<Service/>`, and `<DataLayer/>` components.
		- An `<Envrionment/>` defines a runtime of your app. For instance, you can have a `dev` and a `prod` environment. You can start and deploy each environment separately.
		- A `<Route/>` is a page of your app. it works like the `<Route/>` in `react-router`. [Here’s](https://www.infrastructure-components.com/page?ref=medium_fullstack&dest=https://medium.com/dailyjs/how-to-create-a-navigation-bar-with-react-router-styled-components-and-infrastructure-components-e24bee8d31bb) a tutorial on how to work with the routes.
		- A `<Service/>` defines a function that runs on the server-side. It can have one or multiple `<Middleware/>`-components as children. A `<Middleware/>` works like an Express.js-middleware.
		- The `<DataLayer/>` adds a NoSQL-database to your app. It takes `<Entry/>`-components as children. An `<Entry/>` describes the type of items in your database.
	- These components are all we need to build our full-stack app. As you can see, our exemplary app has:
		1. one runtime environment,
		2. a single page,
		3. two services,
		4. and a database,
		5. with a single entry.
	    The structure of components provides a clear overview of your app. The bigger your app gets, the more important this is.
		You might have noticed that the `<Service/>`s are children of the `<DataLayer/>`. This has a simple reason. We want our services to have access to the database. It really is that easy!
- Database design:
		The `<DataLayer/>` creates an Amazon DynamoDB. This is a key-value database (NoSQL). It delivers high performance at any scale. But unlike relational databases, it does not support complex queries.
		The schema of the database has three fields: `primaryKey`, `rangeKey` and `data`. This is important because you need to know that you can only find entries through its keys. Either the `primaryKey`, the `rangeKey`, or both.





## References
1. https://medium.com/dailyjs/how-to-build-and-deploy-a-full-stack-react-app-4adc46607604
