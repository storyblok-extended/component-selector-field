# Storyblok Extended: Component selector

> A Storyblok field to select a component from the current story.

## Input options

Input options to customize the plugin.

#### version

Type: `'published' | 'draft'`

Sets the version of the story to fetch.

#### path

Type: `string`

Sets the path to the components to filter starting from the story content in dot notation (e.g. `body.my_list`).

#### component

Type: `string`

Sets the name of the components to filter as a comma separated list (e.g. `list_item,magic_list_item`).

#### name

Type: `string`

Sets the path to the components' name to use as display string in dot notation (e.g. `body.my_list`).

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Lints and fixes files

```
npm run lint
```