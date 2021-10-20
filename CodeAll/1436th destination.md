### 
给你一份旅游线路图，该线路图中的旅行线路用数组 paths 表示，其中 paths[i] = [cityAi, cityBi] 表示该线路将会从 cityAi 直接前往 cityBi 。
请你找出这次旅行的终点站，即没有任何可以通往其他城市的线路的城市。
题目数据保证线路图会形成一条不存在循环的线路，因此恰有一个旅行终点站。


示例 1：

输入：paths = [["London","New York"],["New York","Lima"],["Lima","Sao Paulo"]]
输出："Sao Paulo" 
解释：从 "London" 出发，最后抵达终点站 "Sao Paulo" 。本次旅行的路线是 "London" -> "New York" -> "Lima" -> "Sao Paulo" 。
示例 2：

输入：paths = [["B","C"],["D","B"],["C","A"]]
输出："A"
解释：所有可能的线路是：
"D" -> "B" -> "C" -> "A". 
"B" -> "C" -> "A". 
"C" -> "A". 
"A". 
显然，旅行终点站是 "A" 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/destination-city
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
########################

class Solution {
    public String destCity(List<List<String>> paths) {
        HashMap<String,Boolean> destcity = new HashMap<String,Boolean>();
        for(List<String> path:paths){
            for (int i=0;i<path.size();i++){
                if(!destcity.containsKey(path.get(i))){
                    destcity.put(path.get(i), true);
                } else if(destcity.get(path.get(i))){
                    destcity.replace(path.get(i), false);
                }
            }
        }
        for(List<String> path:paths){
            for (int i=0;i<path.size();i++){
                if(destcity.get(path.get(i))&&i!=0){
                    return path.get(i);
                }
            }
        }
        return "wrong";
    }
}

############
自己的思路就是先遍历所有地点，如果遍历时经过了两次，说明这个城市是中转城市，否则为起点或终点。然后再次遍历，返回终点。此方法过于复杂，因为得到了起终点两个结果。
###########
官方解答

class Solution {
    public String destCity(List<List<String>> paths) {
        Set<String> citiesA = new HashSet<String>();
        for (List<String> path : paths) {
            citiesA.add(path.get(0));
        }
        for (List<String> path : paths) {
            if (!citiesA.contains(path.get(1))) {
                return path.get(1);
            }
        }
        return "";
    }
}

#############
将所有路线的起点保存到hashset中，然后所有路线的终点如果不是路线中的起点，说明其为最终目的地。
###########
三叶解法
#########
class Solution {
    public String destCity(List<List<String>> ps) {
        Map<String, String> map = new HashMap<>();
        for (List<String> p : ps) map.put(p.get(0), p.get(1));
        String ans = ps.get(0).get(0);
        while (map.containsKey(ans)) ans = map.get(ans);
        return ans;
    }
}

作者：AC_OIer
链接：https://leetcode-cn.com/problems/destination-city/solution/gong-shui-san-xie-jian-dan-fang-jia-mo-n-y47c/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

#################
将路线保存为hashmap映射关系，如果这个值不是别人的键，说明这个城市不是其他城市的起点，所以就是终点。
