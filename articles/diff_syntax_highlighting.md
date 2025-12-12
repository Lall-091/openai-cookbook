# Diff Syntax Highlighting in Markdown

Diff syntax highlighting is a feature in Markdown that allows you to display code changes with visual indicators showing additions, deletions, and unchanged lines. This is particularly useful for documentation, tutorials, code reviews, and showing before/after comparisons.

## What is Diff Syntax Highlighting?

Diff syntax highlighting uses special formatting to visually distinguish between:
- **Added lines** (typically shown in green with a `+` prefix)
- **Deleted lines** (typically shown in red with a `-` prefix)  
- **Unchanged lines** (shown without a prefix for context)

This mirrors the output format of version control tools like `git diff` and makes code changes easier to understand at a glance.

## How to Use Diff Syntax Highlighting

### Basic Syntax

To create a diff-highlighted code block in Markdown, use triple backticks followed by `diff`:

````markdown
```diff
- This line was removed
+ This line was added
  This line stayed the same
```
````

### Example: Simple Code Change

Here's how a basic function modification would look:

```diff
function greet(name) {
-  return "Hello " + name;
+  return `Hello ${name}!`;
}
```

In this example:
- The old string concatenation is marked as removed (-)
- The new template literal is marked as added (+)
- The function signature remains unchanged

## Common Use Cases

### 1. Showing Code Improvements

Demonstrate refactoring or code improvements:

```diff
// Before: unclear variable names
- const x = getUserData();
- const y = x.filter(u => u.age > 18);
+ // After: descriptive variable names
+ const users = getUserData();
+ const adults = users.filter(user => user.age > 18);
```

### 2. Security Fixes

Highlight security vulnerabilities and their fixes:

```diff
function authenticateUser(username, password) {
-  const query = `SELECT * FROM users WHERE username='${username}' AND password='${password}'`;
-  return db.execute(query);
+  const query = 'SELECT * FROM users WHERE username=? AND password=?';
+  return db.execute(query, [username, password]);
}
```

### 3. Configuration Changes

Show updates to configuration files:

```diff
{
  "name": "my-app",
-  "version": "1.0.0",
+  "version": "1.1.0",
  "dependencies": {
-    "express": "^4.17.1"
+    "express": "^4.18.2"
  }
}
```

### 4. Git Patch Format

For more detailed changes, you can use the `patch` language identifier to show full git-style patches:

````markdown
```patch
diff --git a/server.js b/server.js
index 1234567..abcdefg 100644
--- a/server.js
+++ b/server.js
@@ -10,7 +10,7 @@ const app = express();
 
 app.get('/api/data', (req, res) => {
-  const data = fetchData(req.query.id);
+  const data = await fetchDataSecurely(req.user.id);
   res.json(data);
 });
```
````

## Diff vs Patch

While both are used to show code changes, there are subtle differences:

### `diff` - Simplified Change Display
- Best for showing conceptual changes
- Focuses on what changed
- Cleaner for tutorials and documentation
- Uses simple +/- prefixes

### `patch` - Full Git Patch Format  
- Shows complete git patch information
- Includes file paths, commit hashes, line numbers
- Can be applied with `git apply`
- Better for actual code integration

## Best Practices

### 1. Keep Changes Focused

Show only the relevant changes. Don't include too much unchanged code:

```diff
function processOrder(order) {
  validateOrder(order);
-  const tax = calculateTax(order.total);
+  const tax = calculateTaxWithDiscount(order.total, order.discountCode);
  return finalizeOrder(order, tax);
}
```

### 2. Add Context Comments

Include comments to explain why changes were made:

```diff
// Fix: Prevent race condition in concurrent requests
function updateUserProfile(userId, data) {
-  const user = getUser(userId);
-  user.update(data);
+  const user = await getUserWithLock(userId);
+  await user.update(data);
+  await user.releaseLock();
}
```

### 3. Use for Before/After Comparisons

Great for showing API changes or migrations:

```diff
// Old API (deprecated)
- client.fetchUser(userId, function(err, user) {
-   if (err) handleError(err);
-   processUser(user);
- });

// New API (Promise-based)
+ try {
+   const user = await client.fetchUser(userId);
+   processUser(user);
+ } catch (error) {
+   handleError(error);
+ }
```

### 4. Combine with Language Syntax Highlighting

While diff highlighting is useful, sometimes you want to show the language syntax too. In those cases, show both:

**The Change (with diff):**
```diff
- const result = items.map(i => i.value);
+ const result = items.map(item => item.value);
```

**The Complete Code (with language highlighting):**
```javascript
const items = getItems();
const result = items.map(item => item.value);
console.log(result);
```

## Advanced Techniques

### Showing Multiple File Changes

When documenting changes across multiple files:

```diff
# File: src/config.js
- export const API_URL = "http://localhost:3000";
+ export const API_URL = process.env.API_URL || "http://localhost:3000";

# File: src/client.js
+ import { API_URL } from './config';
  
  function makeRequest(endpoint) {
-   return fetch("http://localhost:3000" + endpoint);
+   return fetch(API_URL + endpoint);
  }
```

### Highlighting Entire Blocks

Use diff to show when entire functions or sections are added or removed:

```diff
+ function newFeature() {
+   // This entire function is new
+   const data = processData();
+   return data;
+ }

- function deprecatedFeature() {
-   // This entire function is being removed
-   return oldImplementation();
- }
```

## What You Can Do with Diff Syntax Highlighting

1. **Documentation**: Clearly show code evolution in tutorials and guides
2. **Code Reviews**: Illustrate changes for review discussions
3. **Migration Guides**: Help users upgrade between versions
4. **Bug Fixes**: Demonstrate security patches and bug resolutions
5. **Refactoring**: Show code improvements and optimizations
6. **Teaching**: Explain programming concepts through incremental changes
7. **Release Notes**: Highlight important API or behavior changes
8. **Pull Request Descriptions**: Provide clear context for code changes

## Limitations and Alternatives

### When Not to Use Diff

- **Complete New Files**: Just use regular code blocks with language highlighting
- **Very Large Changes**: Consider linking to actual git commits or patches
- **Multiple Scattered Changes**: Might be clearer to show separate before/after code blocks

### Alternative Approaches

If diff highlighting doesn't meet your needs:

1. **Side-by-Side Comparison**: Use tables to show before/after
2. **Annotated Code**: Use comments to indicate changes
3. **Separate Code Blocks**: Show "Before" and "After" in distinct blocks
4. **Screenshots**: For UI changes, screenshots may be clearer

## Rendering Support

Diff syntax highlighting is widely supported:

- ✅ GitHub (both issues and README files)
- ✅ GitLab  
- ✅ Bitbucket
- ✅ Most static site generators (Jekyll, Hugo, Next.js)
- ✅ Documentation platforms (GitBook, Read the Docs)
- ✅ Many Markdown editors

Always test your diff blocks in your target platform to ensure proper rendering.

## Conclusion

Diff syntax highlighting is a powerful tool for technical documentation. It makes code changes immediately visible and understandable, helping readers quickly grasp what changed and why. Whether you're writing tutorials, documenting APIs, or explaining bug fixes, diff highlighting can make your documentation clearer and more effective.

Use it whenever you need to show code evolution, compare implementations, or highlight specific changes in a larger codebase.
