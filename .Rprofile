source("renv/activate.R")

# Point reticulate at this project's uv environment (created by `uv sync`),
# so R and Python chunks in the same post share one pinned Python.
# Not normalizePath(): it would follow the symlink out of the venv.
local({
  venv_python <- if (.Platform$OS.type == "windows") ".venv/Scripts/python.exe" else ".venv/bin/python"
  if (file.exists(venv_python)) Sys.setenv(RETICULATE_PYTHON = file.path(getwd(), venv_python))
})
