# How to Generate uv.lock

The `uv.lock` file locks exact dependency versions for reproducibility. It's generated locally by running:

```bash
# In your BA798_TIM directory
uv sync
```

This will create `uv.lock` automatically. Then commit and push it:

```bash
git add uv.lock
git commit -m "Generate uv.lock for reproducible environment"
git push origin assignment-2
```

**Why?** The assignment requires uv.lock to be committed for reproducibility.
