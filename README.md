# Remix + Vite + Storybook + Nx

## Update to latest Nx

```
nx migrate 15.9.0-beta.0
```

This is needed, so that we can use the `nx g @nrwl/storybook:configuration project-name --storybook7Configuration` generator.

The docs are one version ahead, they refer to 15.9.0, which is still in beta. That's why the `storybook7Configuration` flag did not work for you on version `15.8.6`.

## Vite configuration

I ran

```
npx nx g @nrwl/vite:configuration
```

and it indeed did not work. This is because, as you noticed, there were no `targets` in `apps/demo/project.json`. I updated the generator to not throw an error if no targets are present. You could solve this by just adding a `targets` object in your `apps/demo/project.json` file, right below `"tags": [],`. But of course it's better to update the generator.

However, we do not support converting a Remix project to Vite automatically (using the `@nrwl/vite:configuration`), as noted [in the docs](https://nx.dev/packages/vite/generators/configuration#projects-that-can-be-converted-to-use-the-executors). You may try to configure it manually, though!

## Storybook configuration

I ran

```
yarn add -D @nrwl/storybook@15.9.0-beta.0
```

to install the latest version of the Storybook plugin, and then I ran

```
nx g @nrwl/storybook:configuration demo --storybook7Configuration --storybook7UiFramework=@storybook/react-vite
```

(it does not matter what options you pass to the questions the generator asks you afterwards)

and it worked. It generated all the Storybook configuration and added the `storybook` targets to the `demo` project.
