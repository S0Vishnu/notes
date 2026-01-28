

import bpy
import os

# ===== SETTINGS =====
EXPORT_DIR = r"C:\Users\Vishnu\Downloads\horror-game-floor-generator-free"
APPLY_TRANSFORMS = True
# ====================

os.makedirs(EXPORT_DIR, exist_ok=True)

scene = bpy.context.scene
view_layer = bpy.context.view_layer

# Only export mesh objects
objects = [obj for obj in scene.objects if obj.type == 'MESH']

# Store original states
original_locations = {obj.name: obj.location.copy() for obj in objects}
original_visibility = {obj.name: obj.hide_viewport for obj in objects}

for obj in objects:
    # Hide all other objects
    for other in objects:
        other.hide_viewport = (other != obj)
        other.select_set(False)

    # Select active object
    obj.hide_viewport = False
    obj.select_set(True)
    view_layer.objects.active = obj

    # Reset position
    obj.location = (0.0, 0.0, 0.0)

    export_path = os.path.join(EXPORT_DIR, f"{obj.name}.glb")

    bpy.ops.export_scene.gltf(
        filepath=export_path,
        export_format='GLB',
        use_selection=True,
        export_apply=APPLY_TRANSFORMS
    )

    # Restore position
    obj.location = original_locations[obj.name]
    obj.select_set(False)

# Restore visibility
for obj in objects:
    obj.hide_viewport = original_visibility[obj.name]

print(f"Exported {len(objects)} GLB files to:\n{EXPORT_DIR}")
