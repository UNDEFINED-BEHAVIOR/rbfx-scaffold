# Configure engine build. Persistent options here.
set (URHO3D_CSHARP ON CACHE BOOLEAN "" FORCE)
set (URHO3D_SAMPLES OFF CACHE BOOLEAN "" FORCE)

# Include engine in the build
# engine should come first
add_subdirectory(rbfx)
# the rest of internal deps here