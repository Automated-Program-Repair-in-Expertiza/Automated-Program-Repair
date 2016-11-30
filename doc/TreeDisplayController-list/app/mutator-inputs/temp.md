TreeDisplayController#list:/app/mutator-inputs/tree_display_controller.rb:2:a090c
@@ -1,6 +1,6 @@
```
 def list
   if current_user.student?
-    redirect_to(controller: :student_task, action: :list)
+    redirect_to(action: :list)
   end
 end
 ```
