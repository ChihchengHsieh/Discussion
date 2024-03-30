# Discussion

未看先猜, Q* = Q-learning的variantion + A-star, 訓練方式是使用GPT-5來產生Action的Token, 在Action的集裡包含了類似對搜尋引擎或是Vector Index的訪問. 然後是Off-policy的使用現在GPT已經收集的數據 (讓你比較哪個回答比較好): RLHF的延伸. 這個模型應該可以外置化Context信息, 而不是像GPT-3或4將所有的資訊存在FFN層. 所以整個模型的重點會變成RL訓練的穩定性. 

舉個例子: 如果我詢問Q* "請問最近前沿AI的研究情況如何?"

那Q*產生的結果或許會像是:

```python
# 一個LLM將會去自建的VectorIndex或是現有的搜尋引擎搜尋這三個關鍵字, 然後將將搜尋來的訊息依照你的Prompt來整合進scratchpad. 
# Q* 大概率能有一個Action是去調用LLM的.

<Search>hot AI topics<Search><WriteToScratchpad>
<Search>trending AI publications<Search><WriteToScratchpad>
<Search>tredning AI tools<Search><WriteToScratchpad>

# 能產生Sequence of actions 時候, 你就能加入推理這個步驟, 在Scrathpad加入初步的解答(草稿)
<ScratchpadInference>

# 讀取Scratchpad裡的訊息加上Prompt以後產生Output給你
<ReadContextGenerateOutput> 
```

這樣的話, 這個模型就滿足

1. Context外置 (縮小模型大小), 記憶信息從FFN移出以後, 也可以避免Q*去依照現有的資訊產生hallucination.
2. 不是直接的輸出文字, 而是Actions, 那在輸出文字做`<Output>`前, 就可以加入`<Reasoning>`/`<Inference>`這個步驟. 
3. 能產生搜索的Actions, 那就能使用LLM或其他工具去擷取最新的訊息
4. 更廣義的來說上一項: 能產生Actions, 那就可以發送Action給其他Tools. 例如像是現在GPT-4渣爛的大數加減乘除. 原本在GPT-4, 模型必須得在內部進行數學運算, 但如果他能發送準確Actions給計算機這個Tool的時候呢?(`<SendNumberToCalculator>` 或是`<SendOperationToCalculator>`), 這時候LLM只要負責產生命令, 和複製貼上數值而已; LLM最不擅長的精確計算則可以交由特定的工具來完成. 對於複雜的算法來說, LLM只需要列出Equation到Scratchpad以後, 就可以藉由現在一堆的EquationToAnswer的工具來解了.
5. 這裡面很多概非常相似於LangChain裡面面對各種Store的方式.
6. 相對於GPT-4, 這個算法並不是一個飽讀詩書, 滿腹經綸的模型, 但它會是一個好的規劃者, 工作指派者, 工具使用者; 將回答步驟列出後, 讓各個工具(人員)發揮所常; 然後當各個工具(人員), 把報告(Scratchpad)都上交上來後; 它會將資料彙整以後請發言人(<Output>)做出最適當的回答. 
7. OpenAI 以RL的方向去往AGI的方向邁進也很符合大眾的預想, 在非Reward的訓練方式下, 模型最好的擬合結果就是媲美人類的集體知識, 它可以超過個人的Performance, 但很難越過人類的集體知識, 但RL不管在AlphaGO或是OpenAI Five, 都是可超人類群體的. 因為Supervised Learning 或是 Self-Supervised learning都是在從訓連集中學習來模仿人類, 模仿醫生給的診斷, 模仿人類最有可能寫出的下一個字. 而RL是結果論的, 它能靠Exploration去尋找人類並沒有嘗試過的解法, 沒有下過的棋路; 進而迸發超越人類集體的可能性. 
8. 如果我們真的能找到一個穩定通用, 有效運用現有資料的RL算法和訓練方法; 那我們真的離AGI不遠了

抱歉, 講得很亂; 請大家多多來探討自己的見解.
