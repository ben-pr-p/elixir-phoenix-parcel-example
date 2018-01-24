# Elixir/Phoenix/Parcel Example

This app is an example Elixir + Phoenix app with [Parcel](https://parceljs.org/)
as the bundler for front-end assets instead of brunch.

I think Parcel is the bundler that best matches Phoenix's low-configuration
extensibility!

Although this repo has an example web app using parcel, I think it's difficult
to clone this repo and do a bunch of find and replaces for your desired app
and module names. Instead, follow the below steps:

```
mkdir my-directory
mix phoenix.new my-directory --app great_app        # optionally --no-ecto
cd my-directory
```

Now open package.json and delete all the brunch dev dependencies. Then,
```
npm i --save parcel-bundler
```

Note: depending on your deployment strategy (if you're sending your assets to S3
separately, etc.) you may want to `--save-dev`.

Now open package.json and replace the scripts section with:
```
  "scripts": {
    "deploy": "parcel build web/static/js/app.js --out-dir priv/static/js",
    "watch": "parcel watch web/static/js/app.js --out-dir priv/static/js"
  }
```

And open `config/dev.exs` and replace `watchers:` at the end of the first config
block with:
```
watchers: [node: ["node_modules/parcel-bundler/bin/cli.js", "watch", "web/static/js/app.js", "--out-dir", "priv/static/js"]]
```

That's it! Parcel is awesome!

## Babel

Now, to do some React / ES6, just:
```
npm i --save preact
npm i --save babel-preset-env
npm i --save babel-preset-react
```

And `.babelrc` it:
```
{
  "presets": ["env", "react"]
}
```

Or, for full awesomeness, also do:
```
npm i --save babel-preset-stage-0
npm i --save babel-plugin-transform-class-properties
```

and `.babelrc` should be:
```
{
  "presets": ["stage-0", "env", "react"],
  "plugins": ["transform-class-properties"]
}
```

Check out more recipes here: https://parceljs.org/recipes.html. I like Preact.

Everything just works!

## TODO

- [ ] Multiple input -> output entry points
