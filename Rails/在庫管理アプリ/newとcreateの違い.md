### new 

- フォーム(View)に渡すため、空のオブジェクトを作成
	- フォームは渡されたオブジェクトをもとに入力欄を作成
		- `form_with model: @task`をすると
		- `partName:string`という属性があれば`form.text_field :partName`などを自動で書いてくれる

### create

- フォーム(View)から送られてきたデータを使って新しいオブジェクトを作成、DBへ保存
	- `@task = Task.new(task_params)`としてフォームの内容をもとにオブジェクトを作成
	- `@task.save`でデータベースへ保存