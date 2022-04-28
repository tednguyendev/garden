---
{"dg-publish":true,"permalink":"/posts/definition-spec-in-ruby/","tags":"gardenEntry","dgHomeLink":true,"dgPassFrontmatter":false}
---


# Definition Spec in Ruby

![DefinitionSpec](https://res.cloudinary.com/practicaldev/image/fetch/s--bMYwKJHT--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k352hu2x1to4eay8e2p8.png)

Pipelines turn green, but ... the application is lying, Puma does not get up. Fuckup. Clients call, support in panic, Prosecco is gasping.

Wait, but how everything worked locally, how pipelines went through since there was a character in the code that should never be there.

Three issues are to blame for this incident:
-   specificity of interpreted languages
-   specificity of Ruby on Rails
-   lack of adequate, minimum test coverage in the project

## The specificity of interpreted languages
The difference between interpreted languages and compiled languages is that in interpreted languages, the code is executed on the fly, there is no build phase to allow the compiler to check that the project is at all building. Hence, any typos, syntax errors, and undeclared methods will only come to light when the dirty code fragment is executed.

## The specificity of Ruby on Rails
RoR has a lazy loading mechanism in development and test modes. Only in production mode, the entire application code is loaded.

## No minimum test coverage
It just so happened that the flawed class in this example did not have any tests written. If you don't write tests because there is some super important business reason (yes, super important one), it's always worth writing one test that saves your butt in cases like the one above.

## I call it the definition spec.

```ruby
describe MailingService do
  it 'is defined' do
    expect(described_class).not_to be nil
  end
end
```

It consists in the fact that the first test that I write when creating a new class is checking whether such a class is defined at all, which forces it to be loaded in test mode. These are 4 lines, and they really allow you to ensure a minimum of security and avoid typos. **The definition spec in Ruby** seems to be a tool tailored to this problem.