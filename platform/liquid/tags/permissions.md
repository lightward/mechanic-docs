# permissions

The `permissions` tag explicitly declares which Shopify API access scopes a task requires. This gives Mechanic an exact list of the scopes your task needs, instead of relying on automatic detection. Use this tag when you want to be explicit, or when automatic detection is difficult or ambiguous. Task previews are still important for showing users what the task will do and for verifying task behavior. For a broader overview, see [Permissions](../../../core/tasks/permissions.md).

## Usage

```liquid
  {% permissions %}
  read_orders
  write_products
  {% endpermissions %}
```

This declares that your task requires the `read_orders` and `write_products` Shopify API access scopes.

### Declaring multiple permissions

You can list permissions one per line or separate them with commas:

```liquid
  {% permissions %}
  read_orders
  write_products
  read_customers, write_inventory
  read_locations
  {% endpermissions %}
```

Both formats work equally well - choose whichever is most readable for your task.

## Important notes

#### Static analysis only

The permissions tag uses static analysis - it reads your permissions as plain text when saving the task. Liquid code inside the permissions block will not be evaluated:

```liquid
  {% comment %}This will NOT work:{% endcomment %}
  {% permissions %}
  read_orders
  {{ shop.domain }}  {% comment %}Liquid not evaluated!{% endcomment %}
  {% assign scope = "write_products" %}{{ scope }}  {% comment %}Won't work!{% endcomment %}
  {% endpermissions %}
```

#### One permissions block per task

Each task can only have one permissions tag. Multiple permissions tags will cause a validation error.

#### No duplicate permissions

Each permission should be listed only once within the permissions tag.

#### Mechanic validates all permissions when saving a task:

* Invalid permissions: Misspelled or non-existent permissions cause an error
* Disallowed permissions: Some permissions cannot be requested by Mechanic
