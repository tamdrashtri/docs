# Convex Migrations

## What Are Database Migrations?

Database migrations are like careful renovations for your data. Just like you wouldn't tear down a wall in your house without planning, you shouldn't change your database structure without a proper migration strategy[1]. Migrations help you transform data from one format to another while keeping your application running smoothly.

**Why do you need migrations?** Your initial database design is rarely perfect. As you build and learn, you'll want to add new fields, remove old ones, or change how data is stored. Migrations let you make these changes safely without downtime[1].

## Getting Started with Convex Migrations

### **Installation and Setup**

First, install the migrations component in your Convex project:

```bash
npm install @convex-dev/migrations
```

Next, configure your `convex.config.ts` file:

```typescript
// convex/convex.config.ts
import { defineApp } from "convex/server";
import migrations from "@convex-dev/migrations/convex.config";

const app = defineApp();
app.use(migrations);

export default app;
```

### **Basic Initialization**

Create a migrations file (typically `convex/migrations.ts`) and set up the basic structure:

```typescript
import { Migrations } from "@convex-dev/migrations";
import { components } from "./_generated/api.js";
import { DataModel } from "./_generated/dataModel.js";

export const migrations = new Migrations(components.migrations);
export const run = migrations.runner();
```

## Understanding Schema Changes in Convex

Convex makes schema changes easier than traditional databases. You don't need to write "add column" commands - just update your `schema.ts` file and Convex handles the structure changes automatically[1]. However, Convex won't let you deploy a schema that doesn't match your actual data, which is where migrations come in.

**The Safe Migration Process:**
1. **Make fields optional** - First, update your schema to make the field you want to change optional
2. **Update your code** - Modify your application code to handle both old and new data formats
3. **Run the migration** - Transform the actual data
4. **Finalize the schema** - Remove the optional status once all data is migrated

## Common Migration Patterns

### **Adding New Fields with Default Values**

When you want to add a new field to existing documents:

```typescript
export const setDefaultPlan = migrations.define({
  table: "teams",
  migrateOne: async (ctx, team) => {
    if (!team.plan) {
      await ctx.db.patch(team._id, { plan: "basic" });
    }
  },
});
```

**Schema progression:**
- Start: No `plan` field
- Step 1: Add `plan: v.optional(v.union(v.literal("basic"), v.literal("pro")))`
- Step 2: Run migration to set default values
- Step 3: Change to `plan: v.union(v.literal("basic"), v.literal("pro"))`

### **Removing Fields Safely**

To remove a field from your documents:

```typescript
export const removeBoolean = migrations.define({
  table: "teams",
  migrateOne: async (ctx, team) => {
    if (team.isPro !== undefined) {
      await ctx.db.patch(team._id, { isPro: undefined });
    }
  },
});
```

**Important:** Always make fields optional in your schema before removing them from data[1].

### **Changing Field Types**

You can transform data types during migration:

```typescript
export const zipCodeShouldBeAString = migrations.define({
  table: "addresses",
  migrateOne: async (ctx, address) => {
    if (typeof address.zipCode === "number") {
      return { zipCode: address.zipCode.toString() };
    }
  },
});
```

### **Moving Data Between Tables**

Sometimes you need to restructure how data is organized:

```typescript
export const changePreferencesToDocument = migrations.define({
  table: "users",
  migrateOne: async (ctx, user) => {
    const prefs = await ctx.db
      .query("preferences")
      .withIndex("userId", (q) => q.eq("userId", user._id))
      .first();
    if (!prefs) {
      await ctx.db.insert("preferences", user.preferences);
      await ctx.db.patch(user._id, { preferences: undefined });
    }
  },
});
```

## Running Migrations

### **Single Migration Execution**

**From the command line:**
```bash
npx convex run migrations:run '{"fn": "migrations:setDefaultValue"}'
```

**From the dashboard:** Navigate to your functions panel and run the migration function directly.

**Programmatically:**
```typescript
await migrations.runOne(ctx, internal.migrations.setDefaultValue);
```

### **Running Multiple Migrations in Sequence**

For complex changes requiring multiple steps:

```typescript
export const runAll = migrations.runner([
  internal.migrations.setDefaultValue,
  internal.migrations.validateRequiredField,
  internal.migrations.convertUnionField,
]);
```

Then run:
```bash
npx convex run migrations:runAll
```

### **Testing Migrations Safely**

Before running migrations on important data, test them first:

```bash
npx convex run migrations:runIt '{dryRun: true}'
```

This runs one batch and then stops, showing you what would happen without making permanent changes[1].

## Advanced Migration Techniques

### **Handling Large Datasets**

For tables with thousands of documents, migrations run in batches to avoid timeouts and conflicts[1]:

```typescript
export const largeMigration = migrations.define({
  table: "bigTable",
  batchSize: 50, // Process 50 documents at a time
  migrateOne: async (ctx, doc) => {
    // Your migration logic here
  },
});
```

### **Migrating Specific Document Ranges**

You can target only documents that need changes:

```typescript
export const validateRequiredField = migrations.define({
  table: "myTable",
  customRange: (query) =>
    query.withIndex("by_requiredField", (q) => q.eq("requiredField", "")),
  migrateOne: async (_ctx, doc) => {
    return { requiredField: "" };
  },
});
```

### **Parallel Processing**

For independent operations, you can process documents in parallel:

```typescript
export const parallelMigration = migrations.define({
  table: "myTable",
  parallelize: true,
  migrateOne: () => ({ optionalField: "default" }),
});
```

**Warning:** Only use parallel processing when operations don't depend on each other[1].

## Quick Dashboard Migrations

For simple changes in development, you can use the Convex dashboard's bulk edit feature[1]:

1. Navigate to your table in the dashboard
2. Select documents to modify
3. Use the JSON editor to apply patches:

```json
{
  "newField": "defaultValue",
  "fieldToUpdate": true,
  "fieldToRemove": undefined
}
```

**Important:** This approach is recommended mainly for development environments, not production[1].

## Migration Management and Monitoring

### **Checking Migration Status**

Monitor your migrations in real-time:

```bash
npx convex run --component migrations lib:getStatus --watch
```

Or programmatically:
```typescript
const status = await migrations.getStatus(ctx, {
  migrations: [internal.migrations.setDefaultValue],
});
```

### **Stopping Migrations**

If you need to halt a running migration:

```bash
npx convex run --component migrations lib:cancel '{name: "migrations:myMigration"}'
```

### **Restarting Migrations**

To restart from the beginning:

```bash
npx convex run migrations:runIt '{cursor: null}'
```

## Best Practices and Safety Tips

### **Development vs Production**

- **Development:** Use dashboard bulk edits for quick iterations
- **Production:** Always use coded migrations for traceability and safety[1]

### **Schema Validation**

Convex prevents you from deploying schemas that don't match your data. This safety feature ensures your types always reflect reality[1].

### **Data Safety**

- Always test migrations with `dryRun: true` first
- Consider backing up important data before major migrations
- Use optional fields as intermediate steps
- Prefer deprecating fields over deleting them when user data is involved[1]

### **Performance Considerations**

- Start with serial processing; only parallelize if needed
- Use appropriate batch sizes (smaller for large documents, larger for small ones)
- Consider running migrations during low-traffic periods

## Integration with Deployment

You can automate migrations as part of your deployment process:

```bash
npx convex deploy --cmd 'npm run build' && npx convex run migrations:runAll --prod
```

Or create a post-deploy function:

```typescript
export const postDeploy = internalMutation({
  args: {},
  handler: async (ctx) => {
    await migrations.runSerially(ctx, allMigrations);
  },
});
```

## Troubleshooting Common Issues

**Migration won't start:** Check if another migration is already running on the same table.

**Schema validation errors:** Ensure your schema allows for both old and new data formats during the transition.

**Performance issues:** Reduce batch size or add delays between batches.

**Partial failures:** Migrations can resume from where they left off - check the status and restart if needed.

This guide covers the essential concepts and practical techniques for managing database migrations in Convex. Remember that migrations are powerful tools that require careful planning, especially in production environments. Start small, test thoroughly, and always have a rollback plan for critical changes.

[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/57661101/d0842c92-a1f5-425d-8e28-0c2f01a270bf/paste.txt