+++
title = "How to Better Read Open Source Software — Part 1: Introduction to ERD Tools"
date = 2024-03-21
description = "A comprehensive guide on using ERD tools to better understand open source software projects, with practical examples using Rails and Spree."

[taxonomies]
tags = ["Backend", "Programmer", "Open Source", "ERD", "Rails"]

[extra]
show_comments = true
+++

# How to Better Read Open Source Software — Part 1: Introduction to ERD Tools

Reading quality open source software is one of the best ways to significantly improve your coding skills. Open source projects can sometimes be very large and complex, so having techniques to navigate and understand them is essential. While many articles on this topic exist on platforms like Juejin, this series aims to share some of my own experiences to help you get started. This first article introduces ERD (Entity Relationship Diagram) tools.

---

## What is an ER Model?

The ER (Entity Relationship) model is usually the output of system analysis used to define and describe the key content within the business domain. It does **not** define business processes but presents business data patterns in a graphical form.

Typically, the ER model consists of entities (represented by boxes) connected by relationships (lines), where the lines represent associations and dependencies between entities. For example, a building can have zero or more apartments, but an apartment belongs to exactly one building.

Entities can be characterized by relationships as well as additional attributes, including identifiers called *primary keys*. A diagram showing entities, their attributes, and relationships is called an entity-attribute-relationship diagram (ERD).

In practice, ER models are implemented as databases. In a simple relational database, each row represents an instance of an entity type, each column is an attribute, and relationships between entities are implemented via foreign keys.

---

## How ERD Helps in Reading Open Source Projects

ERDs visually represent the data structures and relationships, helping you quickly grasp the backbone of complex systems. This is especially helpful when facing large open source codebases — ERDs highlight the core data models and how they interact, speeding up comprehension.

---

## Example: Generating ERD for Spree E-commerce

Spree is a popular open-source e-commerce platform similar to Shopify. Let's use it as an example to generate an ERD.

```bash
git clone git@github.com:spree/spree_starter.git
cd spree_starter
bundle add rails-erd
bundle
```

On macOS, install Graphviz:

```bash
brew install graphviz
```

Configure your database in `config/database.yaml` and create the database, then run migrations:

```bash
bundle exec rake db:migrate
```

Generate the ERD:

```bash
bundle exec erd --notation=simple --direct --orientation=vertical --splines=ortho --connected --attributes=false
```

You will get a simple diagram showing main entities and their relationships.

---

## Improving ERD with Color

The default ERD can be hard to read with many entities connected together. To improve clarity, change the `rails-erd` gem source in your `Gemfile` to a version that supports colors:

```ruby
gem 'rails-erd', github: "williamhatch/rails-erd"
```

Then update bundles and regenerate:

```bash
bundle
bundle exec erd --notation=simple --direct --orientation=vertical --splines=ortho --connected
```

The color-coded ERD improves readability by visually distinguishing different modules and their relationships.

---

## Next Steps and Future Plans

- Define entities and their relationships in YAML format, then automatically generate Rails models and colored ERDs.
- Create a web-based drag-and-drop UI (inspired by Strapi admin dashboard) for easier YAML and ERD generation.
- Develop a Nuxt3-based UI to select and generate ERDs only for related modules, making diagrams more focused.

The ultimate goal is to enable drag-and-drop modeling that automatically generates RESTful and GraphQL APIs, supports authentication and logging, and follows Domain-Driven Design (DDD) principles in the backend.

---

## References and Resources

- [rails-erd GitHub repository](https://github.com/williamhatch/rails-erd)  
- [Spree Starter GitHub repository](https://github.com/spree/spree_starter)  
- Original article in Chinese on Juejin: [How to Better Read Open Source Software Part 1 (掘金)](https://juejin.cn/post/7037627476897431565)

---

## Visual References

![ERD Overview](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1d0e9fa83dc643f9ba658c96a2f45b8c~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

![Spree ERD Example](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/43f91ab75410462e9dc350ce19f53eb4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

![Colored ERD](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d178a03fae97415c885792f8b1ed4c71~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

![Rails ERD Configuration](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4f919fb40ddc4dbb892121e9842c8587~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

---

Thanks for reading! Feel free to leave a comment or follow me on GitHub for more insights on open source and backend development.
