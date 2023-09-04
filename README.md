
from fastapi import FastAPI, HTTPException

app = FastAPI()

tasks = [
    {"id": 1, "title": "Comprar leite", "done": False},
    {"id": 2, "title": "Pagar contas", "done": False},
]

@app.get("/tasks")
def get_tasks():
    return {"tasks": tasks}

@app.post("/tasks/")
def create_task(task: dict):
    new_task = task
    new_task["id"] = len(tasks) + 1
    tasks.append(new_task)
    return {"message": "Tarefa criada com sucesso!"}

@app.get("/tasks/{task_id}")
def get_task(task_id: int):
    task = next((task for task in tasks if task["id"] == task_id), None)
    if task:
        return task
    raise HTTPException(status_code=404, detail="Tarefa não encontrada")

@app.put("/tasks/{task_id}")
def update_task(task_id: int, updated_task: dict):
    task = next((task for task in tasks if task["id"] == task_id), None)
    if task:
        task.update(updated_task)
        return {"message": "Tarefa atualizada com sucesso!"}
    raise HTTPException(status_code=404, detail="Tarefa não encontrada")

@app.delete("/tasks/{task_id}")
def delete_task(task_id: int):
    task = next((task for task in tasks if task["id"] == task_id), None)
    if task:
        tasks.remove(task)
        return {"message": "Tarefa excluída com sucesso!"}
    raise HTTPException(status_code=404, detail="Tarefa não encontrada")
