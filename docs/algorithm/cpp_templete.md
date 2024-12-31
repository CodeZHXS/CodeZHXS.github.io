## 使用教程

-   在 VSCode 工作区的 `.vscode` 文件夹下创建 `XXX.code-snippets` 文件，其中 `XXX` 表示工作区的名字，例如 `cpp` 工作区，就创建 `cpp.code-snippets`。
-   复制下列内容，粘贴进去。
-   使用 `prefix` 字段的快捷键就可以呼出代码模板，例如 `tpdsu` 就可以输出并查集模板。
-   快速生成新模板，参考 [snippet generator](https://snippet-generator.app/?description=&tabtrigger=&snippet=&mode=vscode)

```json
{
	"C语言默认代码模板": {
		"prefix": "cyy",
		"body": [
			"#include <stdio.h>",
			"#include <string.h>",
			"",
			"$0",
			"",
			"int main()",
			"{",
			"    return 0;",
			"}"
		],
		"description": "C语言默认代码模板"
	},
	"C++默认代码模板": {
		"prefix": "cpp",
		"body": [
			"#include <iostream>",
			"",
			"using namespace std;",
			"$1",
			"int main()",
			"{",
			"\t$0",
			"    return 0;",
			"}"
		],
		"description": "C++默认代码模板"
	},
	"重定向标准输入到文件": {
		"prefix": "tpfreopen",
		"body": [
			"freopen(\"in.txt\", \"r\", stdin);",
		],
		"description": "重定向标准输入到文件"
	},
	"cin快读": {
		"prefix": "tpcin",
		"body": [
			"ios::sync_with_stdio(false);",
			"cin.tie(nullptr);"
		],
		"description": "cin快读"
	},
	"并查集": {
		"prefix": "tpdsu",
		"body": [
			"class DSU",
			"{",
			"public:",
			"    explicit DSU(int n) : parent_or_size(n, -1) {}",
			"",
			"    void merge(int a, int b)",
			"    {",
			"        int x = leader(a), y = leader(b);",
			"        if (x == y)",
			"            return;",
			"        if (-parent_or_size[x] < -parent_or_size[y])",
			"            std::swap(x, y);",
			"        parent_or_size[x] += parent_or_size[y];",
			"        parent_or_size[y] = x;",
			"    }",
			"",
			"    int leader(int a) { return parent_or_size[a] < 0 ? a : parent_or_size[a] = leader(parent_or_size[a]); }",
			"",
			"    bool same(int a, int b) { return leader(a) == leader(b); }",
			"",
			"    int size(int a) { return -parent_or_size[leader(a)]; }",
			"",
			"private:",
			"    vector<int> parent_or_size;",
			"};"
		],
		"description": "并查集(按秩合并)"
	},
	"扩展欧几里得": {
		"prefix": "tpexgcd",
		"body": [
			"pair<LL, LL> exgcd(LL a, LL b)",
			"{",
			"    if (b == 0)",
			"        return {1, 0};",
			"    auto [x, y] = exgcd(b, a % b);",
			"    return {y, x - a / b * y};",
			"}",
			"",
			"LL inv(LL a, LL p)",
			"{",
			"    auto [x, y] = exgcd(a, p);",
			"    if (x < 0)",
			"        x += p;",
			"    return x;",
			"}"
		],
		"description": "扩展欧几里得"
	},
	"TreapSet普通平衡树": {
		"prefix": "tptreap",
		"body": [
			"template <typename K, typename CMP = less<K>>",
			"class TreapSet",
			"{",
			"public:",
			"    void insert(K val)",
			"    {",
			"        auto [x, temp] = split_by_val(root, val - 1);",
			"        auto [y, z] = split_by_val(temp, val);",
			"        if (y != nullptr)",
			"            y->inc();",
			"        else",
			"            y = new Node(val);",
			"        root = merge(merge(x, y), z);",
			"    }",
			"",
			"    void erase(K val)",
			"    {",
			"        auto [x, temp] = split_by_val(root, val - 1);",
			"        auto [y, z] = split_by_val(temp, val);",
			"        y->dec();",
			"        if (y->cnt == 0)",
			"        {",
			"            delete y;",
			"            y = nullptr;",
			"        }",
			"        root = merge(merge(x, y), z);",
			"    }",
			"",
			"    int rank(K val) const",
			"    {",
			"        auto [x, y] = split_by_val(root, val - 1);",
			"        int ans = get_size(x) + 1;",
			"        root = merge(x, y);",
			"        return ans;",
			"    }",
			"",
			"    K kth_element(int k) const",
			"    {",
			"        auto [x, y] = split_by_rank(root, k);",
			"        int ans = x->max_son();",
			"        root = merge(x, y);",
			"        return ans;",
			"    }",
			"",
			"    K prev_element(K val) const",
			"    {",
			"        auto [x, y] = split_by_val(root, val - 1);",
			"        K ans = x->max_son();",
			"        root = merge(x, y);",
			"        return ans;",
			"    }",
			"",
			"    K next_element(K val) const",
			"    {",
			"        auto [x, y] = split_by_val(root, val);",
			"        K ans = y->min_son();",
			"        root = merge(x, y);",
			"        return ans;",
			"    }",
			"",
			"    void debug() const",
			"    {",
			"        auto print = [](auto &self, Node *p) -> void",
			"        {",
			"            if (p == nullptr)",
			"                return;",
			"            self(self, p->lch);",
			"            for (int i = 0; i < p->cnt; i++)",
			"                cout << p->val << ' ';",
			"            self(self, p->rch);",
			"        };",
			"        print(print, root);",
			"        cout << endl;",
			"    }",
			"",
			"private:",
			"    struct Node",
			"    {",
			"        K val;",
			"        int pri;",
			"        int size = 1, cnt = 1;",
			"        Node *lch = nullptr;",
			"        Node *rch = nullptr;",
			"",
			"        Node(int v) : val(v), pri(gen_priority()) {}",
			"",
			"        static int gen_priority()",
			"        {",
			"            static mt19937 gen(random_device{}());",
			"            static uniform_int_distribution<> dis;",
			"            return dis(gen);",
			"        }",
			"",
			"        void push_up() { size = cnt + get_size(lch) + get_size(rch); }",
			"",
			"        void inc()",
			"        {",
			"",
			"            size++;",
			"            cnt++;",
			"        }",
			"",
			"        void dec()",
			"        {",
			"            size--;",
			"            cnt--;",
			"        }",
			"",
			"        int max_son() const",
			"        {",
			"            const Node *p = this;",
			"            while (p->rch != nullptr)",
			"                p = p->rch;",
			"            return p->val;",
			"        }",
			"",
			"        int min_son() const",
			"        {",
			"            const Node *p = this;",
			"            while (p->lch != nullptr)",
			"                p = p->lch;",
			"            return p->val;",
			"        }",
			"    };",
			"",
			"    CMP less;",
			"",
			"    static int get_cnt(Node *p) { return p == nullptr ? 0 : p->cnt; }",
			"",
			"    static int get_size(Node *p) { return p == nullptr ? 0 : p->size; }",
			"",
			"    mutable Node *root = nullptr;",
			"",
			"    // 返回的左子树小于等于 val",
			"    pair<Node *, Node *> split_by_val(Node *p, K val) const",
			"    {",
			"        if (p == nullptr)",
			"            return {nullptr, nullptr};",
			"        if (less(p->val, val))",
			"        {",
			"            auto [x, y] = split_by_val(p->rch, val);",
			"            p->rch = x;",
			"            p->push_up();",
			"            return {p, y};",
			"        }",
			"        else if (less(val, p->val))",
			"        {",
			"            auto [x, y] = split_by_val(p->lch, val);",
			"            p->lch = y;",
			"            p->push_up();",
			"            return {x, p};",
			"        }",
			"        else",
			"        {",
			"            Node *y = p->rch;",
			"            p->rch = nullptr;",
			"            p->push_up();",
			"            return {p, y};",
			"        }",
			"    }",
			"",
			"    // 返回的左子树至少有 k 个元素",
			"    pair<Node *, Node *> split_by_rank(Node *p, int k) const",
			"    {",
			"        if (p == nullptr)",
			"            return {nullptr, nullptr};",
			"        int lch_size = get_size(p->lch);",
			"        int mid_cnt = get_cnt(p);",
			"        if (lch_size + mid_cnt < k)",
			"        {",
			"            auto [x, y] = split_by_rank(p->rch, k - lch_size - mid_cnt);",
			"            p->rch = x;",
			"            p->push_up();",
			"            return {p, y};",
			"        }",
			"        else if (lch_size >= k)",
			"        {",
			"            auto [x, y] = split_by_rank(p->lch, k);",
			"            p->lch = y;",
			"            p->push_up();",
			"            return {x, p};",
			"        }",
			"        else",
			"        {",
			"            Node *y = p->rch;",
			"            p->rch = nullptr;",
			"            p->push_up();",
			"            return {p, y};",
			"        }",
			"    }",
			"",
			"    Node *merge(Node *x, Node *y) const",
			"    {",
			"        if (x == nullptr)",
			"            return y;",
			"        if (y == nullptr)",
			"            return x;",
			"        if (x->pri < y->pri)",
			"        {",
			"            x->rch = merge(x->rch, y);",
			"            x->push_up();",
			"            return x;",
			"        }",
			"        else",
			"        {",
			"            y->lch = merge(x, y->lch);",
			"            y->push_up();",
			"            return y;",
			"        }",
			"    }",
			"};"
		],
		"description": "TreapSet"
	},
	"Treap文艺平衡树": {
		"prefix": "tptreap",
		"body": [
			"class Treap",
			"{",
			"public:",
			"    Treap(int n)",
			"    {",
			"        stack<Node *> st;",
			"        for (int i = 1; i <= n; i++)",
			"        {",
			"            Node *x = new Node(i);",
			"            while (!st.empty() && st.top()->pri >= x->pri)",
			"            {",
			"                st.top()->push_up();",
			"                st.pop();",
			"            }",
			"            if (st.empty())",
			"            {",
			"                x->lch = root;",
			"                root = x;",
			"            }",
			"            else",
			"            {",
			"                Node *y = st.top();",
			"                x->lch = y->rch;",
			"                y->rch = x;",
			"            }",
			"            st.push(x);",
			"        }",
			"        while (!st.empty())",
			"        {",
			"            st.top()->push_up();",
			"            st.pop();",
			"        }",
			"    }",
			"",
			"    void reverse(int l, int r)",
			"    {",
			"        auto [temp, z] = split_by_rank(root, r);",
			"        auto [x, y] = split_by_rank(temp, l - 1);",
			"        flip(y);",
			"        root = merge(merge(x, y), z);",
			"    }",
			"",
			"    void debug()",
			"    {",
			"        auto print = [](auto &self, Node *p) -> void",
			"        {",
			"            if (p == nullptr)",
			"                return;",
			"            p->push_down();",
			"            self(self, p->lch);",
			"            cout << p->val << ' ';",
			"            self(self, p->rch);",
			"        };",
			"        print(print, root);",
			"        cout << endl;",
			"    }",
			"",
			"private:",
			"    struct Node",
			"    {",
			"        int val, pri;",
			"        int size = 1;",
			"        bool flag = false;",
			"        Node *lch = nullptr;",
			"        Node *rch = nullptr;",
			"",
			"        Node(int v) : val(v), pri(gen_priority()) {}",
			"",
			"        static int gen_priority()",
			"        {",
			"            static mt19937 gen(random_device{}());",
			"            static uniform_int_distribution<> dis;",
			"            return dis(gen);",
			"        }",
			"",
			"        void push_up() { size = 1 + get_size(lch) + get_size(rch); }",
			"",
			"        void push_down()",
			"        {",
			"            if (flag)",
			"            {",
			"                flip(lch);",
			"                flip(rch);",
			"                flag = false;",
			"            }",
			"        }",
			"",
			"        void reverse()",
			"        {",
			"            swap(lch, rch);",
			"            flag ^= true;",
			"        }",
			"    };",
			"",
			"    static int get_size(Node *p) { return p == nullptr ? 0 : p->size; }",
			"",
			"    static void flip(Node *p)",
			"    {",
			"        if (p != nullptr)",
			"        {",
			"            p->flag ^= 1;",
			"            swap(p->lch, p->rch);",
			"        }",
			"    }",
			"",
			"    mutable Node *root = nullptr;",
			"",
			"    // 返回的左子树有 k 个元素",
			"    pair<Node *, Node *> split_by_rank(Node *p, int k) const",
			"    {",
			"        if (p == nullptr)",
			"            return {nullptr, nullptr};",
			"        p->push_down();",
			"        int lch_size = get_size(p->lch);",
			"        if (lch_size + 1 == k)",
			"        {",
			"            Node *y = p->rch;",
			"            p->rch = nullptr;",
			"            p->push_up();",
			"            return {p, y};",
			"        }",
			"        else if (lch_size + 1 < k)",
			"        {",
			"            auto [x, y] = split_by_rank(p->rch, k - lch_size - 1);",
			"            p->rch = x;",
			"            p->push_up();",
			"            return {p, y};",
			"        }",
			"        else",
			"        {",
			"            auto [x, y] = split_by_rank(p->lch, k);",
			"            p->lch = y;",
			"            p->push_up();",
			"            return {x, p};",
			"        }",
			"    }",
			"",
			"    Node *merge(Node *x, Node *y) const",
			"    {",
			"        if (x == nullptr)",
			"            return y;",
			"        if (y == nullptr)",
			"            return x;",
			"        if (x->pri < y->pri)",
			"        {",
			"            x->push_down();",
			"            x->rch = merge(x->rch, y);",
			"            x->push_up();",
			"            return x;",
			"        }",
			"        else",
			"        {",
			"            y->push_down();",
			"            y->lch = merge(x, y->lch);",
			"            y->push_up();",
			"            return y;",
			"        }",
			"    }",
			"};"
		],
		"description": "Treap文艺平衡树"
	},
	"Trie树": {
		"prefix": "tptrie",
		"body": [
			"struct Trie",
			"{",
			"    struct Node",
			"    {",
			"        unordered_map<char, Node *> ne;",
			"        bool end;",
			"",
			"        Node *at_or_new(char ch)",
			"        {",
			"            auto it = ne.find(ch);",
			"            if (it == ne.end())",
			"                it = ne.insert({ch, new Node()}).first;",
			"            return it->second;",
			"        }",
			"",
			"        Node *at(char ch) const",
			"        {",
			"            auto it = ne.find(ch);",
			"            return it == ne.end() ? nullptr : it->second;",
			"        }",
			"    };",
			"",
			"    Node *root = new Node();",
			"",
			"    void insert(const string &s)",
			"    {",
			"        Node *p = root;",
			"        for(auto ch : s)",
			"            p = p->at_or_new(ch);",
			"        p->end = true;",
			"    }",
			"",
			"    string search(const string &s)",
			"    {",
			"        Node *p = root;",
			"        for(auto ch : s)",
			"        {",
			"            p = p->at(ch);",
			"",
			"        }",
			"    }",
			"};"
		],
		"description": "Trie树"
	},
	"AC自动机": {
		"prefix": "tpac",
		"body": [
			"struct AC",
			"{",
			"    struct Node",
			"    {",
			"        unordered_map<char, Node *> ne;",
			"        vector<Node *> edge;",
			"        Node *fail;",
			"        vector<int *> pattern_ans;",
			"        int cnt = 0;",
			"",
			"        Node *at_or_new(char ch)",
			"        {",
			"            auto it = ne.find(ch);",
			"            if (it == ne.end())",
			"                it = ne.insert({ch, new Node()}).first;",
			"            return it->second;",
			"        }",
			"",
			"        Node *at(char ch)",
			"        {",
			"            auto it = ne.find(ch);",
			"            if (it == ne.end())",
			"                it = ne.insert({ch, fail->at(ch)}).first;",
			"            return it->second;",
			"        }",
			"    };",
			"",
			"    Node *root = new Node();",
			"",
			"    void insert(int *ans_ptr, const string &s)",
			"    {",
			"        Node *p = root;",
			"        for (auto ch : s)",
			"            p = p->at_or_new(ch);",
			"        p->pattern_ans.push_back(ans_ptr);",
			"    }",
			"",
			"    void search(const string &s)",
			"    {",
			"        Node *now = root;",
			"        for (auto ch : s)",
			"        {",
			"            now = now->at(ch);",
			"            now->cnt++;",
			"        }",
			"",
			"        auto dfs = [&](auto &self, Node *u) -> int",
			"        {",
			"            for (auto v : u->edge)",
			"                u->cnt += self(self, v);",
			"            for (auto p : u->pattern_ans)",
			"                *p += u->cnt;",
			"            return u->cnt;",
			"        };",
			"",
			"        dfs(dfs, root);",
			"    }",
			"",
			"    void build()",
			"    {",
			"        queue<Node *> q;",
			"        for (char ch = 'a'; ch <= 'z'; ch++)",
			"        {",
			"            auto it = root->ne.find(ch);",
			"            if (it != root->ne.end())",
			"            {",
			"                q.push(it->second);",
			"                it->second->fail = root;",
			"                root->edge.push_back(it->second);",
			"            }",
			"            else",
			"                root->ne[ch] = root;",
			"        }",
			"        while (!q.empty())",
			"        {",
			"            Node *u = q.front();",
			"            q.pop();",
			"            Node *f = u->fail;",
			"            for (auto [ch, v] : u->ne)",
			"            {",
			"                Node *f_ch = f->at(ch);",
			"                v->fail = f_ch;",
			"                f_ch->edge.push_back(v);",
			"                q.push(v);",
			"            }",
			"        }",
			"    }",
			"};"
		],
		"description": "AC自动机"
	},
	"无权图": {
		"prefix": "tpgraph",
		"body": [
			"struct Graph",
			"{",
			"    int n;",
			"    vector<vector<int>> edge;",
			"",
			"    Graph(int n) : n(n), edge(n) {}",
			"",
			"    void add(int u, int v) { edge[u].push_back(v); }",
			"};"
		],
		"description": "无权图"
	},
	"有权图": {
		"prefix": "tpgraph",
		"body": [
			"struct Graph",
			"{",
			"    struct Edge",
			"    {",
			"        int to, cost;",
			"    };",
			"",
			"    int n;",
			"    vector<vector<Edge>> edge;",
			"",
			"    Graph(int n) : n(n), edge(n) {}",
			"",
			"    void add(int u, int v, int w) { edge[u].push_back({v, w}); }",
			"};"
		],
		"description": "有权图"
	},
	"Dijkstra最短路": {
		"prefix": "tpdijkstra",
		"body": [
			"struct Graph",
			"{",
			"    struct Edge",
			"    {",
			"        int to, cost;",
			"    };",
			"",
			"    int n;",
			"    vector<vector<Edge>> edge;",
			"",
			"    Graph(int n) : n(n), edge(n + 1) {}",
			"",
			"    void add_edge(int u, int v, int w) { edge[u].push_back({v, w}); }",
			"",
			"    vector<int> dijkstra(int s) const",
			"    {",
			"        struct Node",
			"        {",
			"            int u, d;",
			"            bool operator>(const Node &other) const { return d > other.d; }",
			"        };",
			"",
			"        vector<int> dis(n + 1, numeric_limits<int>::max());",
			"        dis[s] = 0;",
			"        priority_queue<Node, vector<Node>, greater<Node>> heap;",
			"        heap.push({s, 0});",
			"        while (!heap.empty())",
			"        {",
			"            auto [u, d] = heap.top();",
			"            heap.pop();",
			"            if (d > dis[u])",
			"                continue;",
			"            for (auto [v, w] : edge[u])",
			"            {",
			"                if (d + w < dis[v])",
			"                {",
			"                    dis[v] = d + w;",
			"                    heap.push({v, dis[v]});",
			"                }",
			"            }",
			"        }",
			"        return dis;",
			"    }",
			"};"
		],
		"description": "Dijkstra最短路"
	},
	"网络最大流": {
		"prefix": "tpdinic",
		"body": [
			"struct Network",
			"{",
			"    struct Edge",
			"    {",
			"        int to, flow, rev_idx;",
			"    };",
			"",
			"    int n;",
			"    vector<vector<Edge>> edge;",
			"",
			"    Network(int n) : n(n), edge(n + 1) {}",
			"",
			"    void add_edge(int u, int v, int w)",
			"    {",
			"        int ui = edge[u].size();",
			"        int vi = edge[v].size();",
			"        edge[u].push_back({v, w, vi});",
			"        edge[v].push_back({u, 0, ui});",
			"    }",
			"",
			"    int dinic(int s, int t)",
			"    {",
			"        vector<int> dep(n + 1);",
			"        vector<size_t> cur(n + 1);",
			"",
			"        auto bfs = [&]() -> bool",
			"        {",
			"            dep.assign(n + 1, -1);",
			"            cur.assign(n + 1, 0);",
			"            dep[s] = 0;",
			"            queue<int> q;",
			"            q.push(s);",
			"            while (!q.empty())",
			"            {",
			"                int u = q.front();",
			"                q.pop();",
			"                int d = dep[u];",
			"                for (auto e : edge[u])",
			"                {",
			"                    int v = e.to;",
			"                    if (e.flow && dep[v] == -1)",
			"                    {",
			"                        dep[v] = d + 1;",
			"                        q.push(v);",
			"                    }",
			"                }",
			"            }",
			"            return dep[t] != -1;",
			"        };",
			"",
			"        auto dfs = [&](auto &self, int u, int in) -> int",
			"        {",
			"            if (u == t)",
			"                return in;",
			"            int out = 0;",
			"            for (size_t &i = cur[u]; i < edge[u].size(); i++)",
			"            {",
			"                Edge &e = edge[u][i];",
			"                int v = e.to;",
			"                if (dep[v] == dep[u] + 1 && e.flow)",
			"                {",
			"                    Edge &r = edge[v][e.rev_idx];",
			"                    int res = self(self, v, min(in, e.flow));",
			"                    e.flow -= res;",
			"                    r.flow += res;",
			"                    in -= res;",
			"                    out += res;",
			"                    if (!in)",
			"                        break;",
			"                }",
			"            }",
			"            return out;",
			"        };",
			"",
			"        int ans = 0;",
			"        while (bfs())",
			"            ans += dfs(dfs, s, numeric_limits<int>::max());",
			"        return ans;",
			"    }",
			"};"
		],
		"description": "网络最大流"
	},
	"最小费用最大流": {
		"prefix": "tpmcmf",
		"body": [
			"struct Network",
			"{",
			"    struct Edge",
			"    {",
			"        int to, flow, cost, rev_idx;",
			"    };",
			"",
			"    int n;",
			"    vector<vector<Edge>> edge;",
			"",
			"    Network(int n) : n(n), edge(n + 1) {}",
			"",
			"    void add_edge(int u, int v, int f, int c)",
			"    {",
			"        int ui = edge[u].size();",
			"        int vi = edge[v].size();",
			"        edge[u].push_back({v, f, c, vi});",
			"        edge[v].push_back({u, 0, -c, ui});",
			"    }",
			"",
			"    pair<int, int> mcmf(int s, int t)",
			"    {",
			"        vector<int> h(n + 1, numeric_limits<int>::max());",
			"        vector<int> dis(n + 1, numeric_limits<int>::max());",
			"        vector<Edge *> from(n + 1);",
			"",
			"        auto spfa = [&]() -> bool",
			"        {",
			"            h[s] = 0;",
			"            queue<int> q;",
			"            q.push(s);",
			"            while (!q.empty())",
			"            {",
			"                int u = q.front();",
			"                q.pop();",
			"                int d = h[u];",
			"                for (auto e : edge[u])",
			"                {",
			"                    int v = e.to;",
			"                    if (e.flow && d + e.cost < h[v])",
			"                    {",
			"                        h[v] = d + e.cost;",
			"                        q.push(v);",
			"                    }",
			"                }",
			"            }",
			"            return h[t] != numeric_limits<int>::max();",
			"        };",
			"",
			"        auto dijkstra = [&]() -> bool",
			"        {",
			"            struct Node",
			"            {",
			"                int u, d;",
			"                bool operator>(const Node &other) const { return d > other.d; }",
			"            };",
			"",
			"            dis.assign(n + 1, numeric_limits<int>::max());",
			"            dis[s] = 0;",
			"            priority_queue<Node, vector<Node>, greater<Node>> heap;",
			"            heap.push({s, 0});",
			"            while (!heap.empty())",
			"            {",
			"                auto [u, d] = heap.top();",
			"                heap.pop();",
			"                if (d > dis[u])",
			"                    continue;",
			"                for (auto &e : edge[u])",
			"                {",
			"                    int v = e.to, c = e.cost + h[u] - h[v];",
			"                    if (e.flow && d + c < dis[v])",
			"                    {",
			"                        dis[v] = d + c;",
			"                        heap.push({v, dis[v]});",
			"                        from[v] = &e;",
			"                    }",
			"                }",
			"            }",
			"            return dis[t] != numeric_limits<int>::max();",
			"        };",
			"",
			"        int mincost = 0, maxflow = 0;",
			"        spfa();",
			"        while (dijkstra())",
			"        {",
			"            for (int u = 0; u <= n; u++)",
			"                h[u] += dis[u];",
			"            int flow = numeric_limits<int>::max();",
			"            for (Edge *p = from[t]; p;)",
			"            {",
			"                flow = min(flow, p->flow);",
			"                int v = p->to, rev_idx = p->rev_idx;",
			"                Edge *r = &edge[v][rev_idx];",
			"                p = from[r->to];",
			"            }",
			"            for (Edge *p = from[t]; p;)",
			"            {",
			"                int v = p->to, rev_idx = p->rev_idx;",
			"                Edge *r = &edge[v][rev_idx];",
			"                p->flow -= flow;",
			"                r->flow += flow;",
			"                p = from[r->to];",
			"            }",
			"            mincost += h[t] * flow;",
			"            maxflow += flow;",
			"        }",
			"        return {mincost, maxflow};",
			"    }",
			"};"
		],
		"description": "最小费用最大流"
	},
	"莫队查询排序": {
		"prefix": "tpmo",
		"body": [
			"struct Query",
			"{",
			"    int l, r, b, id;",
			"    bool operator<(const Query &other) const { return b != other.b ? b < other.b : (r == other.r ? false : ((b & 1) ^ (r < other.r))); }",
			"};"
		],
		"description": "莫队查询排序"
	}
}
```
