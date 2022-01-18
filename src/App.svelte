<script>
  import { Router, Route } from "svelte-navigator";

	import Index from "./Pages/Index.svelte"
	import Create from "./Pages/Create.svelte"
	import Detail from "./Pages/Detail.svelte"
</script>

<main class="px-40 py-10">
  <Router>
    <Route path="/">
      <Index />
    </Route>
    <Route path="create">
      <Create />
    </Route>
    <Route path="detail">
      <Detail />
    </Route>
  </Router>
</main>
