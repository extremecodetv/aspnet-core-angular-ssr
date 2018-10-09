# aspnet-core-angular-ssr

...and it's not working. :(

I create ASP.NET Core app from angular template. I actualy followed these directions from these [official ssr docs][1]

1. Setting up `ASPNETCORE_Environment` variable
2. Run `dotnet new angular`
3. Modify `Startup.cs` and add `UseSpaPrerendering`
4. Add settings to `.angular-cli.json`
5. Add `tsconfig.server.json`
6. Add `app.server.module.ts`
7. Add `main.server.ts`
8. Modify csproj `BuildServerSideRenderer` option
9. Run `npm i` in `ClientApp` directory

In Development it's work fine. But after production build it work's like in development, ssr not working. Angular continues to run on client. 

My steps to build:

1. Run `dotnet build -c Release -o out`

2. Angular builds without errors (after build in `out` dir, creates 2 angular builds - `dist` and `dist-server` (`dist-server` size is 32Kb)

3. Run `dotnet ssr.dll`

These basic steps for SSR. But I modify `main.ts` and `main.server.ts` files, add these to `extraProviders`
for main.ts

    { provide: 'OS', useValue: 'Client' }

for main.server.ts

    { provide: 'OS', useValue: 'Server' }

and modify `home` component
    import { Component, Inject  } from '@angular/core';

    @Component({
      selector: 'app-home',
      templateUrl: './home.component.html',
    })
    export class HomeComponent {

      os: string = null;

      constructor(@Inject('OS') public osinject: string) {
        console.log(osinject);
        this.os = osinject;
      }
    }


Actualy after build and run my project, i should have seen 'Hello Server', but i see 'Hello Client'.

I tried create 2 times from scratch, tried run in docker container, tried run other projects with ssr (find in github). But it all not working for me

    > dotnet --version
    2.1.403

    > node --version
    v8.11.3

Whats went wrong guys? 


  [1]: https://docs.microsoft.com/en-us/aspnet/core/client-side/spa/angular?view=aspnetcore-2.1&tabs=visual-studio#server-side-rendering
  [2]: https://github.com/extremecodetv/aspnet-core-angular-ssr
