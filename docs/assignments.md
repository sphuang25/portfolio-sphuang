---
title: Assignments
layout: doc

---

<script setup>
  import {data as assignments} from './assignments/assignments.data';
  import { withBase } from 'vitepress';
</script>

# Assignment Pages
<ul v-if="assignments.length > 0">
  <li v-for="assignment of assignments">
    <a :href="withBase(assignment.url)">{{ assignment.frontmatter.title }}</a>
  </li>
</ul>
<p v-else>
  Nothing here yet!
</p>
