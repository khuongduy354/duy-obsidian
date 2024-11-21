2022 07 14 16:24
Status: #concept 
Tags: [[file structure]]
# Layered Structure

## 3 Layer
- Controllers
    
    Separate business and route
    
    - Not to
        
        ```jsx
        route.post('/', async (req, res, next) => {
        
            // This should be a middleware or should be handled by a library like Joi.
            const userDTO = req.body;
            const isUserValid = validators.user(userDTO)
            if(!isUserValid) {
              return res.status(400).end();
            }
        
            // Lot of business logic here...
            const userRecord = await UserModel.create(userDTO);
            delete userRecord.password;
            delete userRecord.salt;
            const companyRecord = await CompanyModel.create(userRecord);
            const companyDashboard = await CompanyDashboard.create(userRecord, companyRecord);
        
            ...whatever...
        
            // And here is the 'optimization' that mess up everything.
            // The response is sent to client...
            res.json({ user: userRecord, company: companyRecord });
        
            // But code execution continues :(
            const salaryRecord = await SalaryModel.create(userRecord, companyRecord);
            eventTracker.track('user_signup',userRecord,companyRecord,salaryRecord);
            intercom.createUser(userRecord);
            gaAnalytics.event('user_signup',userRecord);
            await EmailService.startSignupSequence(userRecord)
          });
        ```
        
    
    ```jsx
    route.post('/', 
        validators.userSignup, // this middleware take care of validation
        async (req, res, next) => {
          // The actual responsability of the route layer.
          const userDTO = req.body;
    
          // Call to service layer.
          // Abstraction on how to access the data layer and the business logic.
          const { user, company } = await UserService.Signup(userDTO);
    
          // Return a response to client.
          return res.json({ user, company });
        });
    ```
    
- Business / Service
    
    ```jsx
    import UserModel from '../models/user';
      import CompanyModel from '../models/company';
    
      export default class UserService {
    
        async Signup(user) {
          const userRecord = await UserModel.create(user);
          const companyRecord = await CompanyModel.create(userRecord); // needs userRecord to have the database id 
          const salaryRecord = await SalaryModel.create(userRecord, companyRecord); // depends on user and company to be created
    
          ...whatever
    
          await EmailService.startSignupSequence(userRecord)
    
          ...do more stuff
    
          return { user: userRecord, company: companyRecord };
        }
      }
    ```
    

## Sources

- Tiers aren’t layers
    
    Mọi người vẫn hay nhầm lẫn giữa tier và layer vì cấu trúc phân chia giống nhau (presentation, bussiness , data). Tuy nhiên, thực tế chúng hoàn toàn khác nhau. Nếu 3 tiers có tính vật lí thì 3 layer có tính logic. Nghĩa là ta phân chia ứng dụng thành các phần (các lớp) theo chức năng hoặc vai trò một cách logic. Các layer khác nhau được thực thi trong 1 phân vùng bộ nhớ của process. Vì thế nên một tier có thể có nhiều layer.
    












--- 
# Refererences 

[What's the difference between "Layers" and "Tiers"?](https://stackoverflow.com/questions/120438/whats-the-difference-between-layers-and-tiers)

[A reminder on "Three/Multi Tier/Layer Architecture/Design" brought to you by my late night frustrations.](http://www.hanselman.com/blog/a-reminder-on-threemulti-tierlayer-architecturedesign-brought-to-you-by-my-late-night-frustrations)

[Bulletproof node.js project architecture 🛡️](https://dev.to/santypk4/bulletproof-node-js-project-architecture-4epf)

[https://github.com/santiq/bulletproof-nodejs](https://github.com/santiq/bulletproof-nodejs)

