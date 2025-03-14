
Деревья DOM и CSSOM вместе объединяются в **дерево рендеринга** (Render Tree).  

*Алгоритм построения дерева рендеринга*   
1) На рассмотрение берутся вершины DOM.
2) Из рассмотрения *исключаются вершины*, которые не видны на странице: `<head>` со своим содержимым, `<script>` и прочие подобные.  
3) Для каждой *оставшейся вершины* находятся соответствующие наборы правил в CSSOM.  
4) Из рассмотрения *исключаются вершины*, которые *скрыты* при помощи *CSS*: имеют *объявление* `dispalay: none`.  
5) Каждая оставшейся вершина DOM вместе со своими наборами правил становится вершиной дерева рендеринга.  

*Вершина дерева рендеринга* называется **объектом рендеринга** (Render Object).  

В браузерах на движке *Gecko дерево рендеринга* может также называться **деревом фреймов** (Frame Tree), а *вершина дерева* - **фреймом** (frame) или **боксом** (box).

После построения дерева рендеринга браузер знает, какие объекты видны на странице и какие стили к ним нужно применить.