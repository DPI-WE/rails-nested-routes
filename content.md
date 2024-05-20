# Nesting Routes in Rails Apps 🪆
In this lesson, we will cover the concept of nesting in Rails applications. We will explore nested routes, providing guidelines on when to use them and examples to illustrate their usage.

## Introduction to Nested Routes
Nesting is a way of organizing and structuring your code to reflect the hierarchical relationships between different resources. In Rails, nesting can be applied to routes to create more intuitive and manageable applications. Nested routes in Rails are used to express a hierarchical relationship between resources. For example, if you have a Project model that has many Tasks, you can nest the routes for tasks within the routes for projects.

### When to Use Nested Routes
- **Hierarchical Data**: When one resource logically belongs to another (e.g., tasks belonging to a project).
- **Scoping**: To scope related resources under their parent resource.
- **Clear URLs**: To generate URLs that reflect the relationships between resources (e.g., `/projects/:project_id/tasks`).

### Example of Nested Routes
Here’s how you can define nested routes in a Rails application:

```ruby
# config/routes.rb
Rails.application.routes.draw do
  resources :projects do
    resources :tasks
  end
end
```

This routing configuration allows you to create URLs like `/projects/:project_id/tasks`, which clearly indicate that tasks belong to a specific project.

### Generated Routes
With the above configuration, Rails will generate the following routes:

```ruby
project_tasks       GET     /projects/:project_id/tasks(.:format)           tasks#index
                    POST    /projects/:project_id/tasks(.:format)           tasks#create
new_project_task    GET     /projects/:project_id/tasks/new(.:format)       tasks#new
edit_project_task   GET     /projects/:project_id/tasks/:id/edit(.:format)  tasks#edit
project_task        GET     /projects/:project_id/tasks/:id(.:format)       tasks#show
                    PATCH   /projects/:project_id/tasks/:id(.:format)       tasks#update
                    PUT     /projects/:project_id/tasks/:id(.:format)       tasks#update
                    DELETE  /projects/:project_id/tasks/:id(.:format)       tasks#destroy
```

### Controller and Views for Nested Routes
Here's an example of how you might set up your TasksController to handle nested routes:

```ruby
# app/controllers/tasks_controller.rb
class TasksController < ApplicationController
  before_action :set_project

  def index
    @tasks = @project.tasks
  end

  def show
    @task = @project.tasks.find(params[:id])
  end

  def new
    @task = @project.tasks.build
  end

  def create
    @task = @project.tasks.build(task_params)
    if @task.save
      redirect_to [@project, @task], notice: 'Task was successfully created.'
    else
      render :new
    end
  end

  def edit
    @task = @project.tasks.find(params[:id])
  end

  def update
    @task = @project.tasks.find(params[:id])
    if @task.update(task_params)
      redirect_to [@project, @task], notice: 'Task was successfully updated.'
    else
      render :edit
    end
  end

  def destroy
    @task = @project.tasks.find(params[:id])
    @task.destroy
    redirect_to project_tasks_path(@project), notice: 'Task was successfully destroyed.'
  end

  private

  def set_project
    @project = Project.find(params[:project_id])
  end

  def task_params
    params.require(:task).permit(:name, :description, :due_date)
  end
end
```

## Guidelines for Using Nested Routes
- **Limit Nesting Depth**: Avoid deep nesting. Typically, one level of nesting is sufficient.
- **Use Only When Necessary**: Only use nested routes when there is a clear hierarchical relationship.

## Quiz

- What is a primary benefit of using nested routes in Rails?
- To make URLs longer.
  - Not correct. The purpose is not to make URLs longer.
- To increase server load.
  - Not correct. Nested routes do not necessarily affect server load.
- To reflect hierarchical relationships between resources.
  - Correct! Nested routes help express hierarchical relationships.
{: .choose_best #benefit_of_nested_routes title="Benefit of Nested Routes" points="1" answer="3"}

- Nested routes should be used for deeply nested resources to reflect all hierarchical relationships.
- True
  - Not correct. Deep nesting should be avoided to keep routes manageable.
- False
  - Correct! Deep nesting is generally discouraged in Rails.
{: .choose_best #deep_nesting title="Deep Nesting in Routes" points="1" answer="2"}

## Conclusion
Nesting in Rails, through nested routes, provides a powerful way to manage complex relationships and forms in your application. By following the guidelines and examples provided, you can implement these features effectively to create intuitive and maintainable applications.

Happy coding! 🚀
