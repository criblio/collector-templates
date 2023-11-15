# Contributing 

We love community contributions! Help us expand this project with your configured collector templates.

Have an idea, but don't know where to start? Open an issue and we'll help you!

## Project Structure

Configurations must be stored in a logical structure:

```text
collectors/
  <type>/
    <id>/
      README.md
      breaker.json
      collector.json
      samples/
        sample1.json
        sample2.json
        ...
```

`<type>` must be one of: `rest`, `database`, `script`, etc.

`<id>` is a simple descriptive name of the collector template.

## Standards

Each template directory must include a README file with a minimum set of details:

1. The author of the Template and any relevant contact information
2. Any changes required to the template (e.g. placeholders) to ensure it functions correctly

The collector template must be included in a file `collector.json`. The template must pass an import to a Cribl Stream instance as committed.

Any relevant event breaker templates must be added to a file `breakers.json`. The event breakers.

## Sample Data

Any sample data included with the template must be placed into a directory: `samples`. Each sample file may have a unique descriptive name, and must be in NDJSON format. Data must be importable to Cribl Stream. Sample files must be under 256 KB in size.

⚠️ Absolutely no sensitive data should be committed to this repo! You are responsible for ensure the complete sanitization of any submitted samples.