<script setup>
import { ref, reactive } from 'vue'

// 角色属性变量
const strengthValue = ref(10) // 体魄
const martialArtsValue = ref(2) // 武德
const personalityValue = ref(4) // 人品
const moodValue = ref(6) // 心情
const perseveranceValue = ref(5) // 恒心
const reputationValue = ref(0) // 声望
const actionPoints = ref(0) // 剩余行为点

// 在任何增加行动点的地方，确保不超过5点的上限
const limitActionPoints = () => {
  if (actionPoints.value > 5) {
    actionPoints.value = 5;
    alert('行动点已达到最大限制5点！');
  }
}
const fundsValue = ref(0) // 资金
const inheritanceValue = ref(0) // 传承度
const traditionPurityValue = ref(100) // 传统纯度
const governmentAttentionValue = ref(0) // 政府关注度
const networkValue = ref(0) // 人脉
const admissionCount = ref(0) // 招生人数
const popularity = ref(0) // 知名度
const wordOfMouth = ref(0) // 口碑

// 界面状态
const showRelationshipMenu = ref(false)
const showArenaMenu = ref(false)

// 任务数据
const availableTasks = reactive([
  // 委托任务
  {
    id: 'cleaning',
    name: '环境维护',
    type: '委托任务',
    description: '武馆有规定，年满6岁的学生要参与武馆的日常维护当中，今日是你当值，你要负责:',
    details: ['洒扫庭院', '擦拭座椅、木桩', '除草'],
    miniGame: '扫灰小游戏',
    gains: [{ type: 'strength', value: 3 }, { type: 'reputation', value: 2 }],
    requiredAge: 6
  },
  {
    id: 'guide',
    name: '文化导览员',
    type: '委托任务',
    description: '这是一个在佛山小有名气的武馆，时常会有游客来你们武馆参观，教练/师父觉得你口齿伶俐，因此他让你去给游客们当讲解员。',
    gains: [{ type: 'strength', value: 3 }, { type: 'reputation', value: 1 }],
    requiredAge: 8
  },
  {
    id: 'performance',
    name: '才艺展演',
    type: '委托任务',
    description: '武术表演是一个很好的宣传渠道，因而教练时常会带着你们出去演出。既是为了赚取武馆的运营经费，也是为了向民众宣传。',
    gains: [{ type: 'reputation', value: 3 }, { type: 'martialArts', value: 2 }, { type: 'popularity', value: 100 }],
    requiredAge: 10
  },
  // 生活娱乐
  {
    id: 'exercise',
    name: '科学锻炼',
    type: '生活娱乐',
    description: '运动可以促进多巴胺的释放，运动过后你大汗淋漓，但感觉浑身都轻松了不少，压力一下子都没了。',
    miniGame: '马里奥类型小游戏',
    gains: [{ type: 'strength', value: 3 }, { type: 'mood', value: 10 }, { type: 'health', value: 2 }],
    requiredAge: 6
  },
  {
    id: 'shopping',
    name: '畅享购物',
    type: '生活娱乐',
    description: '情景选项一：周末雨天前的商业街雨伞选购\n初秋的周末午后，灰蒙蒙的云层压得很低，天气预报说傍晚有雷阵雨。你站在商业街中段，望着街角两个隔路相望的伞摊——旧伞上个月被狂风吹断了伞骨，这次必须挑一把结实又轻便的。左边摊位挂着花花绿绿的折叠伞，红色招牌写着"19.9元买一送一"；右边摊位的伞面素净，价签上"89元/把"的数字旁，贴着手写的"抗风测试合格"标签。',
    choices: [
      {
        text: '选19.9元的"买一送一"',
        result: '你被"两把伞更划算"的念头打动，付了钱拎着两把伞往家走。傍晚雨点落下时，你撑开其中一把，伞骨却在风里"咯吱"作响，没几分钟就弯成了弧形。雨越下越大，伞面边缘开始渗水，裤脚很快湿了一片；更糟的是，伞面上的卡通图案被雨水泡得晕开，墨色顺着伞骨流到手上，活像打翻了调色盘。等雨停后你想找摊主换货，却发现摊位早已空无一人，包装上的"厂家电话"拨过去是忙音——最终，两把变形掉色的伞被你扔进垃圾桶，19.9元只换来两小时的"临时遮雨"。',
        gains: [{ type: 'mood', value: -2 }]
      },
      {
        text: '选89元的"抗风款"',
        result: '你犹豫片刻，还是选了那把稍贵的伞。撑开时能感觉到伞骨的硬度，伞面布料厚实却不笨重，伞柄握着也趁手。傍晚雷阵雨来袭，风裹着雨点砸在伞面上，伞骨稳如磐石，连晃动都很轻微。半年后，这把伞陪你经历了无数次风雨：暴雨天伞面不渗水，强风天伞骨不变形，连伞面上的印花都依旧鲜亮。有次伞柄螺丝松动，你联系品牌客服，对方不仅免费寄来新螺丝，还附了详细的安装说明。如今，这把伞成了你的"可靠搭档"，你摸着光滑的伞柄，忽然明白：有些东西看似"贵"，实则是用一次选择，省去了反复更换的麻烦与浪费。',
        gains: [{ type: 'mood', value: 1 }]
      }
    ],
    lifeLesson: '面对日常用品的选择，"低价凑数"或许能解一时之急，而"优质可靠"的投入，往往能带来更长久的省心与实用。',
    requiredAge: 12
  },
  {
    id: 'meditation',
    name: '正念冥想',
    type: '生活娱乐',
    description: '冥想不是发呆、睡觉，而是超越精神，净化意识，摆脱愚昧无知。\n你放空了自己的意识，渐渐沉入意识的世界......',
    details: ['闭上眼睛，将意识放在眉心，深呼吸，吸气，呼气，吸气，呼气......'],
    gains: [{ type: 'perseverance', value: 3 }, { type: 'mood', value: 5 }, { type: 'wisdom', value: 1 }],
    requiredAge: 8
  },
  // 社区活动
  {
    id: 'firstAid',
    name: '急救小课堂',
    type: '社区活动',
    description: '学习急救技能，包括心肺复苏和海姆立克急救法',
    details: [
      '心肺复苏（CPR）的标准步骤为胸外按压、开放气道、人工呼吸，简称"C-A-B"顺序。',
      '海姆立克急救法（Heimlich Maneuver）是一种用于解除气道异物梗阻的急救技术。'
    ],
    videoContent: true,
    quiz: true,
    gains: [{ type: 'strength', value: 1 }, { type: 'reputation', value: 2 }, { type: 'wisdom', value: 2 }],
    requiredAge: 14
  },
  {
    id: 'civilizedPatrol',
    name: '文明纠察员',
    type: '社区活动',
    description: '你是社区的一名文明纠察员，今天是你出门巡察的日子',
    scene: '晨光小区广场（日常巡查）',
    environment: '清晨的小区广场，有老人晨练、儿童玩耍，地面散落着早餐塑料袋，角落有遛狗未清理的粪便，健身器材上晾晒着衣物。',
    subtasks: [
      {
        name: '劝导遛狗牵绳',
        interaction: '对话选择',
        options: [
          { text: '"您好，遛狗牵绳能保护老人和小孩安全，麻烦牵一下吧？"（温和劝导）', success: true, gains: [{ type: 'reputation', value: 2 }] },
          { text: '"小区规定必须牵绳，不牵绳要罚款的！"（强硬警告）', success: false, gains: [{ type: 'reputation', value: -1 }] },
          { text: '默默递上拾便纸和牵引绳（行动示范）', success: true, gains: [{ type: 'reputation', value: 2 }] }
        ]
      },
      {
        name: '清理散落垃圾',
        interaction: '迷你游戏：限时1分钟"垃圾分类投放"',
        miniGame: '拖动不同垃圾（塑料瓶、纸巾、果皮）到对应垃圾桶',
        successCondition: '正确率≥80%',
        successGains: [{ type: 'reputation', value: 2 }],
        failGains: [{ type: 'reputation', value: -1 }]
      },
      {
        name: '制止器材晾晒行为',
        interaction: '道具使用',
        options: [
          { text: '"文明宣传手册"（展示社区公约）', success: true, gains: [{ type: 'reputation', value: 1 }] },
          { text: '"爱心衣架"（提供公共晾晒区指引）', success: true, gains: [{ type: 'reputation', value: 2 }] }
        ]
      }
    ],
    gains: [{ type: 'personality', value: 3 }, { type: 'reputation', value: 2 }],
    requiredAge: 16
  },
  {
    id: 'legalClass',
    name: '法律微课堂',
    type: '社区活动',
    description: '学习法律知识，提升法律意识',
    videoContent: true,
    quiz: true,
    gains: [{ type: 'wisdom', value: 3 }, { type: 'reputation', value: 1 }],
    requiredAge: 16
  }
])

// 关系系统数据
const relationshipEvents = reactive({
  '师父': [
    {
      name: '功法的分歧',
      choices: [
        {
          text: '向师傅请教分歧',
          description: '师傅赞赏你的求真精神，亲自点拨。',
          gains: [{ type: 'relationship', target: '师父', value: 10 }]
        },
        {
          text: '按大师兄方法尝试',
          description: '你另辟蹊径，虽有些冒险但颇有收获。',
          gains: [{ type: 'relationship', target: '大师兄', value: 10 }, { type: 'martialArts', value: 1 }]
        }
      ]
    },
    {
      name: '独门绝技的传授',
      choices: [
        {
          text: '直接向师傅表明强烈学习意愿，并承诺刻苦练习',
          description: '师傅欣赏你的勇气，将拳法传授给你。',
          gains: [{ type: 'relationship', target: '师父', value: 5 }, { type: 'martialArts', value: 5 }, { type: 'mood', value: 5 }]
        },
        {
          text: '每日提前打扫练功场，并为师傅泡好他最喜欢的茶，默默表现',
          description: '数日后师傅察觉你的用心，在一个清晨将你唤至一旁，不仅开始传授拳法，还额外讲解了他当年的实战心得。',
          gains: [{ type: 'relationship', target: '师父', value: 15 }, { type: 'martialArts', value: 3 }, { type: 'mood', value: 3 }]
        }
      ]
    }
  ],
  '大师兄': [
    {
      name: '师傅的偏爱',
      choices: [
        {
          text: '主动请师傅也多指点大师兄',
          description: '师傅深感你心胸宽广。而大师兄虽表面客气，但内心觉得受了羞辱，并在日后修炼中更易与你较劲。',
          gains: [{ type: 'relationship', target: '师父', value: 3 }, { type: 'relationship', target: '大师兄', value: -10 }]
        },
        {
          text: '坦然接受这份偏爱，更加专注地修炼',
          description: '你的武艺精进神速。大师兄彻底失望，认为你工于心计。',
          gains: [{ type: 'relationship', target: '大师兄', value: -10 }]
        }
      ]
    },
    {
      name: '默契的试炼',
      choices: [
        {
          text: '主动配合大师兄节奏',
          description: '演示行云流水，师傅大悦。',
          gains: [{ type: 'relationship', target: '大师兄', value: 10 }, { type: 'relationship', target: '师父', value: 5 }]
        },
        {
          text: '坚持自己的节奏',
          description: '你和师兄的配合毫无默契，多次演示失败。',
          gains: [{ type: 'relationship', target: '大师兄', value: -5 }, { type: 'personality', value: -1 }]
        }
      ]
    }
  ],
  '大师姐': [
    {
      name: '宗门的重担',
      choices: [
        {
          text: '主动帮师姐分担文书工作',
          description: '大师姐感激你的体贴，关系深化。',
          gains: [{ type: 'relationship', target: '大师姐', value: 15 }]
        },
        {
          text: '鼓励她量力而行',
          description: '大师姐感到安慰，但独自承担。',
          gains: [{ type: 'relationship', target: '大师姐', value: 8 }]
        }
      ]
    },
    {
      name: '往日的心结',
      choices: [
        {
          text: '静静聆听陪伴',
          description: '大师姐敞开心扉，信任大增。',
          gains: [{ type: 'relationship', target: '大师姐', value: 20 }]
        },
        {
          text: '理性分析劝解',
          description: '大师姐欣赏你的冷静，但心结未解。',
          gains: [{ type: 'relationship', target: '大师姐', value: 10 }]
        }
      ]
    }
  ],
  '小师弟': [
    {
      name: '师弟的烦恼',
      choices: [
        {
          text: '分享自己的心得',
          description: '师弟茅塞顿开，对你无比感激。',
          gains: [{ type: 'relationship', target: '小师弟', value: 10 }, { type: 'personality', value: 2 }]
        },
        {
          text: '鼓励他自行领悟',
          description: '师弟得到了你的鼓励，恢复了一些信心。',
          gains: [{ type: 'relationship', target: '小师弟', value: 5 }]
        }
      ]
    },
    {
      name: '师弟的生辰',
      choices: [
        {
          text: '精心准备他提及的礼物',
          description: '师弟非常感动，关系显著升温。',
          gains: [{ type: 'relationship', target: '小师弟', value: 20 }]
        },
        {
          text: '召集同门简单庆祝',
          description: '师弟开心，享受热闹氛围。',
          gains: [{ type: 'relationship', target: '小师弟', value: 12 }]
        }
      ]
    }
  ],
  '小师妹': [
    {
      name: '受挫哭泣',
      choices: [
        {
          text: '耐心开导示范',
          description: '小师妹破涕为笑，无比依赖你。',
          gains: [{ type: 'relationship', target: '小师妹', value: 15 }]
        },
        {
          text: '赠予小礼物安慰',
          description: '小师妹情绪好转，感到惊喜。',
          gains: [{ type: 'relationship', target: '小师妹', value: 10 }]
        }
      ]
    },
    {
      name: '好奇惹出祸',
      choices: [
        {
          text: '帮忙善后并温和告诫',
          description: '小师妹既感激又内疚，并保证下次不调皮了。',
          gains: [{ type: 'relationship', target: '小师妹', value: 15 }]
        },
        {
          text: '带她离开并分享趣事',
          description: '帮小师妹转移注意力，小师妹心情好转。',
          gains: [{ type: 'relationship', target: '小师妹', value: 10 }]
        }
      ]
    }
  ],
  '藏书阁馆长': [
    {
      name: '长老的考验——拼图小游戏',
      choices: [
        {
          text: '耐心整理并分类',
          description: '长老对你刮目相看，破例允许你翻阅珍藏武功典籍。',
          gains: [{ type: 'relationship', target: '藏书阁馆长', value: 10 }, { type: 'personality', value: 2 }]
        },
        {
          text: '只快速整理指定区域',
          description: '任务完成，但长老认为你心浮气躁，不予借书。',
          gains: [{ type: 'relationship', target: '藏书阁馆长', value: -3 }]
        }
      ]
    },
    {
      name: '破损的秘典',
      choices: [
        {
          text: '立即坦白过失，并承诺修复',
          description: '长老见你诚实，叹口气拿出修复工具教你方法，但未来一段时间需每日花费时间修复残卷。',
          gains: [{ type: 'mood', value: -5 }]
        },
        {
          text: '试图掩饰，将责任推给"潮湿的天气"',
          description: '长老凭借经验一眼识破，对你大失所望，你短期内被禁止进入藏书阁。',
          gains: [{ type: 'mood', value: -8 }, { type: 'relationship', target: '藏书阁馆长', value: -10 }, { type: 'personality', value: -5 }]
        }
      ]
    }
  ]
})

// 擂台比武系统数据
const combatArena = reactive({
  arenas: [
    {
      id: 'beginner',
      name: '初级擂台',
      requiredAge: 8,
      actionCost: 1,
      rewards: {
        win: [{ type: 'strength', value: 10 }, { type: 'martialArts', value: 8 }, { type: 'perseverance', value: 2 }, { type: 'reputation', value: 3 }],
        lose: [{ type: 'strength', value: -15 }, { type: 'martialArts', value: -5 }, { type: 'perseverance', value: -3 }, { type: 'reputation', value: -3 }]
      },
      styles: [
        {
          name: '洪拳',
          skills: [
            { id: 'gongzi-fuhu', name: '工字伏虎拳' }
          ]
        },
        {
          name: '蔡李佛拳',
          skills: [
            { id: 'chansi-malugiao', name: '缠丝马辘桥' }
          ]
        },
        {
          name: '咏春',
          skills: [
            { id: 'xiaoniantou', name: '小念头' }
          ]
        }
      ]
    },
    {
      id: 'intermediate',
      name: '中级擂台',
      requiredAge: 12,
      actionCost: 1,
      rewards: {
        win: [{ type: 'strength', value: 10 }, { type: 'martialArts', value: 8 }, { type: 'perseverance', value: 2 }, { type: 'reputation', value: 3 }],
        lose: [{ type: 'strength', value: -15 }, { type: 'martialArts', value: -5 }, { type: 'perseverance', value: -3 }, { type: 'reputation', value: -3 }]
      },
      styles: [
        {
          name: '洪拳',
          skills: [
            { id: 'gongzi-fuhu', name: '工字伏虎拳' },
            { id: 'huhe-shuangxing', name: '虎鹤双形拳' }
          ]
        },
        {
          name: '蔡李佛拳',
          skills: [
            { id: 'zuixian-jingjiu', name: '醉仙敬酒' },
            { id: 'chansi-malugiao', name: '缠丝马辘桥' }
          ]
        },
        {
          name: '咏春',
          skills: [
            { id: 'xiaoniantou', name: '小念头' },
            { id: 'biaozhi', name: '标指' }
          ]
        }
      ]
    },
    {
      id: 'advanced',
      name: '高级擂台',
      requiredAge: 16,
      actionCost: 1,
      rewards: {
        win: [{ type: 'strength', value: 10 }, { type: 'martialArts', value: 8 }, { type: 'perseverance', value: 2 }, { type: 'reputation', value: 3 }],
        lose: [{ type: 'strength', value: -15 }, { type: 'martialArts', value: -5 }, { type: 'perseverance', value: -3 }, { type: 'reputation', value: -3 }]
      },
      styles: [
        {
          name: '洪拳',
          skills: [
            { id: 'gongzi-fuhu', name: '工字伏虎拳' },
            { id: 'huhe-shuangxing', name: '虎鹤双形拳' },
            { id: 'tiexian-quan', name: '铁线拳' }
          ]
        },
        {
          name: '蔡李佛拳',
          skills: [
            { id: 'zuixian-jingjiu', name: '醉仙敬酒' },
            { id: 'chansi-malugiao', name: '缠丝马辘桥' },
            { id: 'shangma-gongqiao', name: '上马攻桥' }
          ]
        },
        {
          name: '咏春',
          skills: [
            { id: 'xiaoniantou', name: '小念头' },
            { id: 'biaozhi', name: '标指' },
            { id: 'xunqiao', name: '寻桥' }
          ]
        }
      ]
    }
  ]
})

// 游戏状态
const gameState = reactive({
  currentAge: 1,
  currentStoryIndex: 0,
  selectedChoice: null,
  showChoiceResult: false,
  unlockedFeatures: [],
  relationships: {
    '师父': 5,
    '大师兄': 0,
    '大师姐': 0,
    '小师弟': 0,
    '小师妹': 0,
    '藏书阁馆长': 0
  },
  // 剧情分支状态
  branches: {
    isStayInGuan: false // 是否选择了留在馆内的分支
  },
  // 特殊权限
  permissions: {
    hasIndependentOperation: false // 是否有独立运营权限
  },
  // 任务相关状态
  currentTask: null,
  taskResult: null,
  showTasks: false,
  selectedTaskChoice: null,
  selectedSubtask: null,
  subtaskResults: null,
  currentTaskType: null, // 当前任务类型
  // 藏书阁相关状态
  showLibrary: false, // 是否显示藏书阁
  currentLibraryAction: null, // 当前选择的藏书阁动作
  libraryResult: null, // 藏书阁结果
  // 武术技能熟练度
  skillProficiency: {
    '工字伏虎拳': 0,
    '铁线拳': 0,
    '虎鹤双形拳': 0,
    '锁步': 0,
    '五郎八卦棍': 0,
    '护环短刀': 0,
    '醉仙敬酒': 0,
    '缠丝马辘桥': 0,
    '上马攻桥': 0,
    '单飞脚': 0,
    '走生马': 0,
    '蝴蝶双刀': 0,
    '十字梅花双刀': 0,
    '小念头': 0,
    '寻桥': 0,
    '标指': 0,
    '内钳阳马': 0,
    '侧撑腿': 0,
    '八斩刀': 0,
    '六点半棍': 0
  },
  // 已学技能
  learnedSkills: [],
  // 关系和擂台事件
  currentEvent: null,
  // 历史事件
  history: []
})

// 角色数据
const character = reactive({
  name: '黄飞熊',
  avatar: '🐻',
  lifeStory: [
    {
      age: 1,
      content: '你降生在南派洪拳武术的发源地——佛山鸿胜武馆。晨光透过木格花窗，将父亲带领弟子练拳的身影投在斑驳的砖墙上，弟子们练洪拳的呼喝声如潮水般浸透你的童年。母亲是武馆传人之一，她总在练功间隙将你揽入怀中，襁褓间混杂着檀香与汗水的独特气息，成为你记忆里最安心的味道。',
      gains: [{ type: 'martialArts', value: 1 }, { type: 'mood', value: 10 }]
    },
    {
      age: 2,
      content: '你扶着八仙桌腿摇摇晃晃站起来，指着墙上父亲贴的黄飞鸿海报咿呀学语，小手攥着母亲递来的糖葫芦，跌跌撞撞扑进父亲怀里，他脸上的胡子硌得你咯咯直笑，然后他把你举过头顶，在夕阳里转成一团模糊的光晕。',
      gains: [{ type: 'martialArts', value: 1 }, { type: 'personality', value: 1 }]
    },
    {
      age: 3,
      content: '在幼儿园的沙坑里，你能单手提动装满沙子的铁皮桶；同龄孩子还在玩积木时，你已经能把木马上的小伙伴驮着跑。阿姨们总捏着你肉乎乎的胳膊笑："这娃怕不是铁打的！"某次邻居家的大狼狗冲过来，你竟张开双臂把它逼退三步，从此"佛山小霸王"的名号在巷弄间传开。',
      gains: [{ type: 'strength', value: 2 }, { type: 'reputation', value: 1 }, { type: 'mood', value: 3 }]
    },
    {
      age: 4,
      content: '背着印着"好好学习"的帆布书包走进巷尾的幼儿园，你成了孩子们的"小头目"：午睡时把自己的小被子分给尿床的同桌，滑梯上总让着女生，却在男生抢玩具时一把将木枪夺回来——"要打架？先赢我这招黑虎掏心！"（其实只是胖乎乎的小拳头）。',
      gains: [{ type: 'martialArts', value: 2 }, { type: 'personality', value: 1 }, { type: 'reputation', value: 1 }]
    },
    {
      age: 5,
      content: '父亲蹲下来帮你系好练功服腰带，母亲把熬了整夜的鸡蛋塞进你兜里："去训练场跟师兄师姐们学学规矩，别再把邻居家的鸡追得飞上屋顶了。"武馆的门槛被岁月磨得发亮，师傅眯眼打量你："扎三个月马步，能站稳再说。"',
      gains: [{ type: 'strength', value: 1 }],
      afterGameGains: [{ type: 'martialArts', value: 1 }, { type: 'strength', value: 1 }],
      isGame: true,
      gameName: '基础考察——马步平衡',
      gameDescription: '烈日下扎马步时，汗水滴进青砖缝里洇出深色痕迹，师傅用藤条轻敲你颤抖的膝盖："站如松，不是站如风中柳。"\n游戏设置：玩家操控人物在直径0.3米的"梅花桩"（传统武术基础训练器械）上保持"马步桩"姿势60秒，通过实时调整重心抵御多重干扰，最终达成"桩功入定"状态。\n注：左偏为左偏马，右偏为右偏马，中间为中正马'
    },
    {
      age: 6,
      content: '晨练时看着师兄们打拳的虎虎生风，你攥紧拳头偷偷模仿，却被师傅用烟杆敲了敲手背："根基不牢，地动山摇。马步再扎半年，桩功加练一个时辰。"',
      gains: [{ type: 'strength', value: 1 }],
      choices: [
          {
          text: '偷师，跟着师兄学',
          content: '趁师傅午休时溜到后院，跟着师兄们比划"三路长拳"，却因发力过猛扯到韧带，疼得眼泪在眼眶里打转。师傅发现后没骂你，只是用药酒揉着你起的脚踝："习武先习忍，冒进是大忌。"',
          gains: [{ type: 'strength', value: -3 }]
        },

          {
          text: '主动询问师傅何时可以学习拳法',
          content: '你鼓起勇气拦住师傅："什么时候能学打拳？"他放下手里的茶盏："等你马步扎得能让猫在背上睡觉，再说。"那天的夕阳把你的影子拉得很长，心里有点发酸。',
          gains: [{ type: 'mood', value: -20 }]
        },
        {
          text: '继续基础训练',
          content: '你把师傅的话刻在心里，每天比别人早到一个时辰，桩功站到双腿发麻时，就盯着院角的蚂蚁搬家转移注意力。三个月后，师傅突然说："今日桩功，你第一个达标。"',
          gains: [{ type: 'strength', value: 2 }, { type: 'mood', value: 5 }]
        }
      ]
    },
    {
      age: 7,
      content: '师傅终于掀开泛黄的拳谱："今日教你洪家拳入门式——弓步插掌。"你跟着师傅的口令出拳，拳风扫过耳畔时，惊飞了院角老槐树上的麻雀，掌心火辣辣的疼，心里却像揣了团火。',
      gains: [{ type: 'strength', value: 2 }],
      isGame: true,
      gameName: '学习拳法',
      gameDescription: '师傅握着你的手腕调整角度："力从地起，拳由心发。"你突然觉得丹田处有股暖流涌动——拳法熟练度+10%，累计10次可获得技能。'
    },
    {
      age: 8,
      content: '你的"三路长拳"已打得有模有样，馆里要选五人参加庙会表演。晨练时师傅站在台阶上问："谁愿去？"',
      choices: [
        {
          text: '第一个举手报名',
          content: '你踮着脚尖举高手臂："我去！"师傅看了你一眼点头，其他师兄们也都笑了。表演当天，你穿着绣着金边的练功服，站在最前面，虽然紧张得手心出汗，但每一拳都打得虎虎生风。散场后，一位白胡子老人摸着你的头："此子骨骼清奇，是块练武的好料子。"',
          gains: [{ type: 'reputation', value: 2 }, { type: 'mood', value: 8 }]
        },
        {
          text: '犹豫片刻后举手',
          content: '你看了看师兄们，大家都没有动。当你慢慢举起手时，师傅露出满意的笑容："飞熊愿意去，很好。"表演结束后，师傅单独指导了你几招，你的拳法精进不少。',
          gains: [{ type: 'martialArts', value: 2 }, { type: 'reputation', value: 1 }]
        },
        {
          text: '躲在师兄们后面',
          content: '你缩了缩脖子，藏在大师兄身后。师傅最终选了其他四位师兄，但他临走前看了你一眼，眼神里有些失望。那天晚上，你躲在被子里哭了很久。',
          gains: [{ type: 'mood', value: -15 }]
        }
      ]
    },
    {
      age: 9,
      content: '师傅取来器械架上的齐眉棍："今日教你‘五郎八卦棍’基础——运星。"你托着木棍练了三个月，终于能让棍梢缠住飘落的柳叶。（特色技【棍法·缠丝】解锁，体魄+1）',
      gains: [{ type: 'strength', value: 1 }],
      afterGameGains: [{ type: 'martialArts', value: 1 }, { type: 'strength', value: 1 }],
      isGame: true,
      gameName: '特色技动作游戏——棍法·缠丝',
      gameDescription: '运星：动作要领：左脚向后移一小步，同时棍尖逆时针绕一圈；接着右脚向后移动一步成马步，棍尖乘势向对方腰部点出，高与肋平。\n【特色技动作游戏】\n游戏设置：玩家通过模拟“棍梢缠绕柳叶”的传统武术意象，在60秒内用齐眉棍缠绕飘落的柳叶，积累“缠丝熟练度”，达成条件后解锁特色技【棍法·缠丝】，同步提升角色“体魄”属性。'
    },
    {
      age: 9,
      content: '某天放学，暮色里的巷口飘着炒河粉的香气，却看见高年级学生把你的同桌按在墙上抢零花钱。你攥紧书包带想跑，却想起师傅说的“止戈为武”——',
      choices: [
        {
          text: '使用学到的棍法进行武力压制',
          content: '你抽出藏在书包里的木尺（模仿棍法）横扫过去，却因用力过猛打中对方胳膊，两人都疼得蹲在地上。警察来的时候，你低着头不敢看师傅——他罚你抄《武德论》十遍，却在夜里给你受伤的指关节涂药酒。',
          gains: [{ type: 'strength', value: -2 }, { type: 'personality', value: -2 }]
        },
        {
          text: '报警并上前制止',
          content: '你摸出藏在书包侧袋的儿童电话手表，按下了110。警察叔叔赶到时，你正张开双臂挡在同桌身前，像只炸毛的小狮子。师傅知道后，往你碗里多加了块红烧肉：“勇而有谋，这才是武者该有的样子。”',
          gains: [{ type: 'personality', value: 1 }, { type: 'mood', value: 5 }, { type: 'reputation', value: 1 }]
        },
        {
          text: '多一事不如少一事，假装没有看见',
          content: '你低着头快步走过，却听见身后传来同桌的哭声。第二天他没来上学，听说被打得住了院。你攥着他送你的弹珠，心里像堵了块石头。',
          gains: [{ type: 'personality', value: -3 }, { type: 'mood', value: -10 }]
        }
      ]
    },
    {
      age: 13,
      content: '八年习武，你的拳风里渐渐有了“劲”。晨练时能把“三路长拳”打得虎虎生风，木人桩上的拳印深了半分，师傅说你“拳里有了劲，还差三分巧”。今天演武场上挂起了红绸——岭南少年武术大赛的报名表，就放在师傅的茶桌上。',
      gains: [{ type: 'martialArts', value: 2 }, { type: 'strength', value: 2 }]
    },
    {
      age: 15,
      content: '师傅在祖师爷画像前点燃三炷香，你跪得膝盖发麻，听着他念“武德为先，技为末”的训诫。当他把那本边角磨破的《洪家拳谱》交到你手上时，你突然发现师傅的头发比去年白了些。“从今日起，你是我的第八个徒弟。”',
      afterGameGains: [{ type: 'personality', value: 2 }, { type: 'mood', value: 5 }, { type: 'martialArts', value: 1 }],
      isGame: true,
      gameName: '拜师礼',
      gameDescription: '【触发拜师礼】（人品+2，师父好感度+5，解锁藏书阁学习进阶功法资格）\n可前往藏书阁学习进阶功法——工字伏虎拳、虎鹤双形拳、铁线拳'
    },
    {
      age: 16,
      content: '师父正式传你“五郎八卦棍”，你对着棍谱练了半个月，对“五郎八卦”总不得要领。每天在操场练棍，都觉得动作哪里不对——',
      choices: [
        {
          text: '请教他人',
          content: '你抱着棍谱蹲在大师兄的房门口，大师兄给你讲授练棍要领，你依言比划，原本滞涩的“五郎八卦棍”忽如圆月贯空，棍尖划弧似八卦流转。窗外月光洒落，正照亮师兄鞋帮磨破的旧痕。',
          gains: [{ type: 'martialArts', value: 2 }, { type: 'mood', value: 5 }]
        },
        {
          text: '自己继续琢磨',
          content: '你把棍谱贴在床头，睡前盯着“五郎八卦”四个字发呆，甚至在课堂上用手指比划运棍路线。某天梦中突然顿悟，第二天晨练时一棍击碎了木桩表层——师父说：“痴儿，总算开窍了。”',
          gains: [{ type: 'martialArts', value: 3 }, { type: 'mood', value: -10 }]
        },
        {
          text: '放弃学习拳法',
          content: '你把棍谱塞回书架最底层，谎称“学业太忙”缺席晨练。师父没再逼你，只是那本棍谱的位置，再也没动过。',
          gains: [{ type: 'personality', value: -3 }, { type: 'martialArts', value: -3 }]
        }
      ]
    },
    {
      age: 17,
      content: '高考倒计时牌挂在教室前方，你的黑眼圈越来越重。晨练时扎马步总晃，师父看在眼里，没说什么——',
      choices: [
        {
          text: '暂停训练，以学业为重',
          content: '你鼓起勇气对师父说想专心备考，他从樟木箱底翻出个铁盒子，里面是你五岁时扎马步的照片：“读书是好事，武馆的门永远为你开着。”他把盒子塞进你书包，掌心的老茧蹭得你脸颊发痒。',
          gains: [{ type: 'mood', value: 5 }]
        },
        {
          text: '学业与训练兼顾',
          content: '你每天凌晨四点起床练拳，晚上刷题到深夜，直到某天在课堂上晕倒。医生说你过度疲劳，师父来医院时，手里提着保温桶：“身体是习武的本钱，别本末倒置了。”',
          gains: [{ type: 'strength', value: -2 }]
        }
      ]
    },
    {
      age: 19,
      content: '你考上了省城的大学，临行前师父往你包里塞了本线装的《洪拳拳谱》：“晨练别断，寒暑假回来，我要检查的。”',
      choices: [
        {
          text: '平时在学校晨练，并利用寒暑假回武馆训练',
          content: '你在大学操场的梧桐树下扎马步，寒风吹得衣服猎猎作响。寒暑假回武馆时，师父用尺子量你拳印的深度，眯眼笑：“没偷懒，骨头又硬了三分。”',
          gains: [{ type: 'strength', value: 2 }, { type: 'martialArts', value: 1 }]
        },
        {
          text: '沉迷于游戏娱乐，渐渐忘了师父的嘱托',
          content: '你把《洪拳拳谱》扔在宿舍抽屉，每天和室友开黑到深夜。某次视频通话，师父问：“多久没晨练了？”你含糊其辞，挂了电话后，看着镜子里虚胖的自己，有点陌生。',
          gains: [{ type: 'strength', value: -3 }, { type: 'mood', value: -2 }]
        }
      ]
    },
    {
      age: 22,
      content: '大学毕业你回了佛山，推开武馆门时愣住了——青石板缝里长了草，演武场的木桩裂了缝，师兄们大多外出打工，只剩下几个老头在角落里喝茶。师父看见你，眼睛亮了亮：“回来啦？”',
      choices: [
        {
          text: '招商引资，减轻师父的负担',
          content: '你联系上做文旅的同学，他建议把武馆改造成“武术主题民宿”。签约那天，你看着门楣上“武德堂”的匾额，心里有点空落落的。',
          gains: [{ type: 'personality', value: 1 }, { type: '资金', value: 20 }]
        },
        {
          text: '深入社区推广武术文化',
          content: '你带着师妹在小区公园、老年活动中心免费教授“洪拳养生操”，起初被调侃“花拳绣腿”，但见退休教师李伯用你教的拳法缓解了腰疾，街坊们渐渐围拢过来。半年后社区送来锦旗，武馆报名人数增加12人。',
          gains: [{ type: 'reputation', value: 2 }, { type: '人脉', value: 1 }]
        }
      ]
    },
    {      age: 23,      content: '你的拳法小有所成，可破师兄的“桥手环”。师父说：“打赢你大师兄，就准你出师。”那场比试打了三个时辰，最后你用“工字虎伏拳”震落了师兄的武器——师父把你的名字刻在了“出师名录”上：“以后的路，自己选。”',      choices: [        {          text: '留在馆内，帮助师父管理武馆',          content: '你跪在师父面前磕了三个头：“我不走，我要陪您把武馆传承下去。”他眼眶红了，转身去厨房给你煮面，手有点抖。',          gains: [{ type: '武德', value: 3 }, { type: '师父好感度', value: 10 }],          onSelect: () => {            gameState.branches.isStayInGuan = true;          }        },        {          text: '自立门户，让武术更好的传承下去',          content: '你在新区租了个小店面，挂起“振邦武馆”的牌子。开业那天，师父送来块牌匾，上面写着：“守正创新”。',          gains: [{ type: '人脉', value: 3 }],          onSelect: () => {            gameState.permissions.hasIndependentOperation = true;          }        }      ]    },    {      age: 24,      content: '你在新区租下了一间临街铺面，前任老板留下的“五金店”招牌还没拆干净，你踩着梯子刷墙时，师妹在下面递油漆桶：“师兄，咱们这‘振邦武馆’，能有人来吗？”师父送来的“守正创新”牌匾被你挂在正中央，钉子敲下去时，手心沁出了汗。你手上启动资金仅5万元（父母积蓄+出师红包），需解决场地修缮、器械采购、招生宣传三大难题。',      condition: () => gameState.permissions.hasIndependentOperation,      choices: [        {          text: '压缩成本，租旧仓库改造。',          content: '在城郊工业区租下300平米旧仓库，自己动手刷墙、焊器械架，暴雨天屋顶漏雨，你和师妹用塑料布盖着新做的木人桩，夜里打地铺听着雨声商量招生。',          gains: [{ type: '资金', value: -20000 }]        },        {          text: '贷款租临街旺铺。',          content: '向银行贷款10万元租下步行街二楼铺面，装修时用玻璃墙展示学员练功，路过的年轻人纷纷驻足。但每月还贷压力让你不得不接低价商演（如商场促销表演），师妹抱怨“刚离开老馆就成了卖艺的”。',          gains: [{ type: '心情', value: -20 }, { type: '声望', value: -2 }]        },        {          text: '借力社区资源，共享场地。',          content: '你找到社区主任，提出“免费教老人太极+低价招收青少年”，换取社区活动中心每周三、六的使用权。首堂课有12个老人带着马扎来，其中退休警察张叔说：“我孙子要是敢来，我天天盯着他练！”',          gains: [{ type: '声望', value: 5 }, { type: '招生人数', value: 5 }]        }      ]    },    {      age: 26,      content: '两年过去，武馆终于有了20名稳定学员，但新区同类武馆已有5家，竞争渐烈。你站在镜子前看着自己磨破的练功鞋，想起师父说的“练武先练心，开馆先立信”——',      condition: () => gameState.permissions.hasIndependentOperation,      choices: [        {          text: '专精实战格斗，走“硬核”路线。',          content: '推出“防身术特训营”，学员多为夜班女工、外卖骑手，你亲自演示“被持刀抢劫如何夺刀”，某次帮学员追回被抢手机，本地新闻报道了“武馆师父见义勇为”，但被同行举报“宣扬暴力”。',          gains: [{ type: '声望', value: 2 }, { type: '武德', value: 3 }]        },        {          text: '主打亲子武术，家庭式教学。',          content: '设计“父子拳”“母女剑”课程，家长和孩子一起扎马步、练套路，期末举办“家庭武术节”，给表现最好的家庭发“武学世家”奖状。单亲妈妈李姐说：“以前孩子沉迷游戏，现在天天拉着我练‘虎鹤双形拳’。”',          gains: [{ type: '声望', value: 7 }, { type: '武德', value: 2 }]        },        {          text: '挖掘传统武术文化，做“非遗传承”标签。',          content: '你整理老馆传下的拳谱，开设“洪家拳历史课”，带学员参观佛山武术博物馆，甚至复原了清代“武状元”的甲胄用于表演。文化局领导视察时说：“这才是年轻人该做的传承。”获得“非遗传承示范点”称号。',          gains: [{ type: '传承度', value: 10 }, { type: '政府关注', value: 2 }, { type: '声望', value: 10 }, { type: '知名度', value: 400000 }]        }      ]    },    {      age: 28,      content: '武馆步入正轨时，某短视频平台MCN机构找上门：“签约我们，包装你成‘功夫网红’，年收入保底百万。”对方递来的合同上写着“每月拍10条‘挑战传统武术’剧本视频”（如“洪拳能否打过自由搏击”）——',      condition: () => gameState.permissions.hasIndependentOperation,      choices: [        {          text: '接受签约，流量变现。',          content: '按剧本和“业余搏击选手”拍“打假视频”，视频播放量破亿，线上课程销量暴涨，武馆扩张到3家分店。但某天直播时，有老学员弹幕质问：“师父，你教我们的‘武德’，就是配合剧本演戏吗？”老学员流失80%，师父托人捎来一句话：“水能载舟，亦能覆舟。”',          gains: [{ type: '武德', value: -10 }, { type: '传承度', value: -10 }, { type: '口碑', value: -5 }]        },        {          text: '拒绝签约，深耕线下口碑。',          content: '你撕了合同，转头带着学员去山区支教，教留守儿童练拳强身。孩子们用树枝当剑、石头当桩，你发的支教视频意外在本地火了，标题是《没有拳馆的武术课》。回城后，报名电话被打爆，其中有位家长说：“我想让孩子学的，就是你教的那种‘站如松’。”获得“青年公益人物”称号，师父专程来看你，摸了摸新馆的木人桩：“这才是‘振邦武馆’该有的样子。”',          gains: [{ type: '口碑', value: 10 }, { type: '武德', value: 8 }, { type: '人品', value: 10 }, { type: '知名度', value: 800000 }]        },        {          text: '自制真实武术内容。',          content: '你拒绝剧本，但同意拍“真实武术日常”：教老人防诈骗的“脱身术”、拆解电影武术动作的“武侠冷知识”、学员从零基础到劈砖的成长记录。一年后账号粉丝50万，评论区最多的留言是：“原来武术不是花架子，是真能保护自己的本事。”',          gains: [{ type: '口碑', value: 5 }, { type: '传承度', value: 15 }, { type: '声望', value: 8 }, { type: '知名度', value: 100000 }]        }      ]    },    {      age: 33,      content: '十年过去，“振邦武馆”已成为佛山新区的老字号，你拿着师父当年送你的《洪拳拳谱》，想起二十三岁那个雨天',      condition: () => gameState.permissions.hasIndependentOperation    },    {      age: 35,      content: '振邦武馆在新区站稳脚跟，学员突破200人，师妹们提议：“趁现在名气大，开放加盟，三年开到十家分馆！”你想起师父送的“守正创新”牌匾，心里却打鼓——上周刚发现有加盟商打着“振邦武馆”的旗号，教“速成搏击”，还卖高价“武功秘籍”。',      condition: () => gameState.permissions.hasIndependentOperation,      choices: [        {          text: '快速加盟，统一品牌。',          content: '制定加盟手册，要求分馆统一装修、统一收费，但允许自主教学。一年内开了5家分馆，营收翻番，但有分馆为抢学员，偷偷教“踢裆插眼”的阴招。某次学员家长投诉：“孩子学了你们的‘防身术’，把同学打进医院！”你去分馆问责，馆长却说：“现在家长就认这个，你不教，别人教！”',          gains: [{ type: '声望', value: -6 }, { type: '心情', value: -15 }, { type: '口碑', value: -2 }]        },        {          text: '直营扩张，亲自带教。',          content: '用武馆利润在邻区开两家直营分馆，亲自培训教练，每周巡馆检查。新馆教练都是跟你五年以上的弟子，教拳前先背《武德论》。有分馆教练想接商演，你说：“可以去，但得穿练功服，打全套洪家拳，告诉人家这不是‘表演’，是‘功夫’。”两年后，三家分馆的学员都知道：“振邦武馆的招牌，比奖杯值钱。”',          gains: [{ type: '口碑', value: 5 }, { type: '声望', value: 10 }, { type: '武德', value: 8 }, { type: '传承度', value: 10 }]        },        {          text: '聚焦单馆，深耕社区。',          content: '放弃扩张，把精力放在“振邦武馆·总馆”，开设“家长课堂”“老年太极队”“社区防身讲座”，甚至在馆内设“邻里互助角”，帮居民修家电、看孩子。有个单亲爸爸说：“我送儿子来学拳，不光是练身体，是想让他看看，什么是‘男人的样子’——像你这样，守着个馆，守着群人，踏实。”',          gains: [{ type: '口碑', value: 3 }, { type: '声望', value: 5 }, { type: '武德', value: 5 }, { type: '人品', value: 4 }]        }      ]    },    {      age: 40,      content: '武馆的海外学员越来越多，有个在美国的华人弟子阿明，想在纽约开“振邦武馆分馆”，却来信问：“美国学员不跪祖师爷，行不行？他们觉得‘拜师礼’像宗教仪式。”你想起师父说的“习武先习礼”，又想起阿明发来的照片——美国学员用拖把杆当剑，在车库里练“五郎八卦棍”，笑得一脸灿烂。',      condition: () => gameState.permissions.hasIndependentOperation,      choices: [        {          text: '严格要求，保留传统礼仪。',          content: '回信说“拜师必须跪祖师爷，抱拳礼必须右手握拳左手掌”，否则不准挂“振邦武馆”的牌子。阿明最终妥协，但美国学员流失大半，只剩几个华人孩子。某天视频通话，阿明说：“师父，他们觉得这不是‘学拳’，是‘被管教’。”你看着屏幕里空荡荡的馆，心里有点堵。',          gains: [{ type: '心情', value: -20 }, { type: '声望', value: -4 }, { type: '传承度', value: -5 }]        },        {          text: '完全本土化，适应海外文化。',          content: '允许阿明取消拜师礼，用“握手+鞠躬”代替，甚至把“洪家拳”改叫“Chinese Kung Fu Fitness”。分馆生意火爆，学员上千，但某次阿明发来教学视频——学员打“虎鹤双形拳”时摇头晃脑，像跳街舞。你突然想起师父说的“拳是人的影子”，这些学员的影子里，没有洪家拳的“劲儿”。',          gains: [{ type: '传承度', value: -6 }, { type: '知名度', value: 100000 }]        },        {          text: '核心不变，形式灵活。',          content: '回信告诉阿明：“拜师礼可以改，但‘武德三问’不能少——学拳为了什么？强了欺负人怎么办？忘了初心怎么办？抱拳礼可以用‘右手拇指扣左手虎口’的简化版，但要告诉学员‘这是中国人的‘敬’，敬对手，敬功夫’。”半年后，阿明发来视频：美国学员练完拳，对着祖师爷画像行简化抱拳礼，齐声念“武德三问”，虽然口音蹩脚，却字字认真。',          gains: [{ type: '传承度', value: 8 }, { type: '口碑', value: 2 }, { type: '武德', value: 2 }]        }      ]    },    {      age: 45,      content: '某体育产业集团找上门，提出“1000万收购振邦武馆51%股权”，承诺“三年内上市，让洪家拳走进资本市场”，条件是“更换管理层，引入职业经理人，开设‘少儿武术IPO班’”。合同里有一条：“需配合集团拍摄‘功夫明星养成记’真人秀，学员可签约出道。”',      condition: () => gameState.permissions.hasIndependentOperation,      choices: [        {          text: '接受收购，拥抱资本。',          content: '签字那天，集团老总握着你的手：“以后你就是‘振邦武术集团’的副总裁！”武馆改名为“振邦功夫学院”，学员统一穿印有LOGO的练功服，每周参加真人秀拍摄。有个学员哭着说：“我不想拍节目，我想练拳。”你想安慰他，却被经纪人拦住：“镜头在呢，说点正能量的！”师父留下的《洪拳拳谱》被锁进保险柜，成了“镇馆文物”。',          gains: [{ type: '资金', value: 500000 }, { type: '心情', value: -5 }]        },        {          text: '拒绝收购，坚守独立。',          content: '撕了合同，集团老总冷笑：“等着瞧，不出三年你这馆就得关门！”你转头把武馆的招牌擦得更亮，带着学员去公园打拳，免费教流浪汉防身术。有个曾是投资人的学员说：“我投过那么多项目，就数你这馆‘不赚钱，却值钱’——值在‘干净’。”三年后，集团因“虚假宣传”破产，而振邦武馆的学员，多了50个。',          gains: [{ type: '招生人数', value: 50 }, { type: '口碑', value: 4 }, { type: '声望', value: 2 }]        },        {          text: '部分合作，保留核心。',          content: '与集团谈判：接受200万注资，用于“传统武术研究中心”，但武馆管理权、教学内容、人事任免权归你，拒绝真人秀和IPO班。集团为了“非遗概念”同意了，研究中心成立那天，你带着弟子们整理拳谱，把“洪家拳发力原理”做成力学模型，连大学物理系都来参观。',          gains: [{ type: '传承度', value: 5 }, { type: '口碑', value: 2 }]        }      ]    },    {      age: 60,      content: '你渐渐老去，振邦武馆的继承人成了难题：儿子小远（二十五岁）留学归来，学的是“数字传媒”，对练拳没兴趣，却擅长运营短视频账号；大弟子阿豪（三十岁）传统派，马步扎得比你稳，却连微信都用不利索；二弟子小雅（二十八岁）女弟子，创新派，能把“洪家拳”编成健身操，却被老学员说“不正宗”。',      condition: () => gameState.permissions.hasIndependentOperation,      choices: [        {          text: '传给儿子，拥抱数字时代。',          content: '让小远接手，关闭线下传统班，全力做“振邦武术APP”，推出“AI私教”“虚拟师父对战”。APP下载量破百万，但有老学员卸载时留言：“这不是振邦武馆，是‘振邦游戏厅’。”你看着儿子在发布会上说“要让洪家拳走向元宇宙”，突然觉得师父送的牌匾，有点晃眼。',          gains: [{ type: '传承纯度', value: -50 }]        },        {          text: '传给大弟子，坚守传统。',          content: '把武馆交给阿豪，要求“不准用手机拍拳，不准线上教学，只教能扎三年马步的真徒弟”。阿豪做得很好，传统班保留下来，但学员越来越少，多是中老年人。某天你看见小雅带着一群年轻人在公园练“简化洪家拳”，心里像被什么撞了一下——原来武馆的门，不该只朝一个方向开。',          gains: [{ type: '传承度', value: 2 }, { type: '招生人数', value: -30 }]        },        {          text: '成立“武馆理事会”，三人共治。',          content: '让小远负责“数字传播”，拍真实教学视频、运营海外账号；阿豪负责“传统教学”，带核心弟子练拳谱、打比赛；小雅负责“创新课程”，开发“亲子拳”“女子防身术”。理事会每月开会，吵得面红耳赤，但决议总能平衡传统与现代——小远的视频里，阿豪演示“虎鹤双形拳”的慢动作；小雅的课程里，学员要先背《武德论》；阿豪教传统班时，会用小远做的3D动画讲解“工字伏虎拳”。',          gains: [{ type: '传承度', value: 70 }, { type: '口碑', value: 15 }, { type: '声望', value: 8 }]        }      ]    },    {      age: 75,      content: '你躺在武馆后院的摇椅上，看小远带着海外弟子视频连线，阿豪教孩子们扎马步，小雅给社区老人演示“虎鹤双形拳”。阳光落在“守正创新”的牌匾上，金字被岁月磨得温润，像你手上的老茧。曾孙爬过来，抓着你手里的《洪拳拳谱》咿呀学语，你把他抱进怀里，指着扉页师父的照片：“这是太爷爷的师父，他说‘拳是人的影子，你站得直，影子就正’。”曾孙似懂非懂，用小手拍着拳谱，发出“咚咚”的声音，像极了那年你在老馆扎马步时，心跳的声音。你走时，嘴角带着笑，手里攥着那本师父送的《洪拳拳谱》，书页间夹着三十三岁那年开馆时的油漆刷——刷毛早已磨平，却沾着振邦武馆最初的颜色。许多年后，佛山人仍会说：“振邦武馆的拳，能打，能练，还能暖人心——那才是真功夫。”而武馆的青砖墙上，你的照片旁边，新添了小远、阿豪、小雅的合影，他们身后，是一群扎着马步的孩子，像一片正在生长的森林，根扎在土里，叶向着光，永远年轻，永远滚烫。',      condition: () => gameState.permissions.hasIndependentOperation    },    {
      age: 27,
      content: '你和师兄们把武馆重新刷了漆，演武场的裂缝填上了新土。为了让武馆活下去，你们商量后决定——',
      condition: () => gameState.branches.isStayInGuan,
      choices: [
        {
          text: '组织多场商演',
          content: '接了地产开盘、商场庆典的表演，师妹们穿着亮片演出服打拳时，你总想起小时候师父说的“习武不是卖艺”。但票房分成到账那天，你给武馆换了新的练功垫。',
          gains: [{ type: '资金', value: 150000 }, { type: '知名度', value: 100000 }, { type: '武德', value: -1 }]
        },
        {
          text: '去广场招生',
          content: '你带着师妹们在广场舞队伍旁打拳，大妈们看得热闹，纷纷把孙子送来学“防身术”。有个小男孩总缠着你问：“师父，真的能飞檐走壁吗？”你笑着把他举过头顶：“先把马步扎稳，就能‘飞’起来。”',
          gains: [{ type: '招生', value: 25 }, { type: '声望', value: 5 }, { type: '武德', value: 1 }]
        },
        {
          text: '开拓其他业务，如跌打',
          content: '你请老中医坐馆，推出“武术+理疗”套餐，扭伤脚踝的快递小哥、腰酸背痛的白领挤满了诊室。某天有个病人说：“你们这不仅能治病，还能学拳，真好。”',
          gains: [{ type: '资金', value: 50000 }, { type: '人脉', value: 2 }, { type: '武德', value: 1 }, { type: '声望', value: 10 }]
        }
      ]
    },
    {
      age: 30,
      content: '武馆渐渐有了生气，但知道“洪家拳”的年轻人还是太少——',
      condition: () => gameState.branches.isStayInGuan,
      choices: [
        {
          text: '主动参与各种比赛，让武术被大家看到',
          content: () => {
            if (gameState.stats.strength > 55 && gameState.skills.martialArts.some(skill => skill.Proficiency === 100)) {
              return '你报名了“全国传统武术锦标赛”，决赛时用“五郎八卦棍”击败了北派高手。颁奖台上，你举着写有“佛山洪家拳”的牌子，台下闪光灯一片。';
            } else if (gameState.stats.strength < 55 && gameState.stats.morality < 30) {
              return '你报名了“全国传统武术锦标赛”，高手众多，你有些不敌，但是此前你私下收集了各家对手的弱点之处。最终你还是击败了对手，但不太光彩。';
            } else if (gameState.stats.strength < 55 && gameState.stats.morality > 30) {
              return '你报名了“全国传统武术锦标赛”，但高手众多，你被对手击败，最终无缘八强。';
            }
            return '你报名了“全国传统武术锦标赛”，但结果未知。';
          },
          gains: () => {
            if (gameState.stats.strength > 55 && gameState.skills.martialArts.some(skill => skill.Proficiency === 100)) {
              return [{ type: '知名度', value: 500000 }, { type: '政府关注', value: 1 }, { type: '武德', value: 2 }, { type: '声望', value: 10 }, { type: '心情', value: 5 }];
            } else if (gameState.stats.strength < 55 && gameState.stats.morality < 30) {
              return [{ type: '知名度', value: 200000 }, { type: '武德', value: -3 }, { type: '声望', value: -1 }];
            } else if (gameState.stats.strength < 55 && gameState.stats.morality > 30) {
              return [{ type: '知名度', value: 100000 }, { type: '武德', value: 1 }, { type: '体魄', value: -2 }, { type: '心情', value: -10 }];
            }
            return [];
          }
        },
        {
          text: '与当地学校合作开展义务教学宣讲',
          content: '你带着拳谱走进中小学，给孩子们讲“黄飞鸿抗倭”的故事，教他们简单的防身术。有个小女孩说长大想当“女侠客”，你把自己的第一把木剑送给了她。',
          gains: [{ type: '传承度', value: 20 }, { type: '政府补贴', value: 50000 }, { type: '武德', value: 2 }]
        },
        {
          text: '顺应潮流，拍摄短视频',
          content: '师妹用手机拍你拆解“洪家拳”发力技巧，视频意外火了，评论区有人问：“能线上教学吗？”你犹豫了很久，还是决定开直播——镜头前，你依然穿着练功服。',
          gains: [{ type: '线上学员', value: 300 }, { type: '资金', value: 80000 }, { type: '传统纯度', value: -5 }, { type: '声望', value: 4 }]
        }
      ]
    },
    {
      age: 33,
      content: '武馆的青砖墙上，新添了许多东西——有你和学员们的合影，有比赛获奖的奖牌，还有社区送来的锦旗。某天师父指着照片里那个扎马步的小男孩：“像不像你小时候？”阳光透过窗棂，落在你和他的笑脸上，拳谱的纸页在风里沙沙作响，像极了那年惊飞麻雀的翅膀。',
      condition: () => gameState.branches.isStayInGuan
    },
    {
      age: 35,
      content: '武馆的青砖墙上，除了老照片，多了块“市级非遗传承基地”的铜牌。这天市文化局送来一份文件：邀请武馆参与“国家非遗数字化工程”，将洪家拳的拳谱、招式、心法做成3D动画和VR体验，预算充足，但要求“适配年轻用户，可适当娱乐化改编”。师兄们争论不休——有人说“这是让祖宗拳谱变游戏”，有人说“再不跟上，年轻人连洪家拳的名字都忘了”。',
      condition: () => gameState.branches.isStayInGuan,
      choices: [
        {
          text: '完全配合，技术主导',
          content: '将拳谱、招式交给专业团队，允许他们加入“虚拟师父对战”“招式闯关”等游戏化设计。VR体验上线后，年轻人戴着头显“打拳”，数据显示“洪拳”搜索量暴涨300%，但有老拳师骂：“连‘气沉丹田’都改成‘能量条’，是对祖宗的亵渎！”',
          gains: [{ type: '知名度', value: 300000 }, { type: '传统纯度', value: -5 }, { type: '政府关注', value: 2 }, { type: '声望', value: 3 }]
        },
        {
          text: '拒绝技术改编，只提供资料',
          content: '你只提交拳谱复印件和手写心法笔记，拒绝任何3D或VR改编：“洪家拳是练出来的，不是看出来的。”项目组最终只做了静态网页展示，访问量寥寥。某天你看见邻居家小孩对着手机玩“功夫游戏”，随口说“这拳打得比洪拳好看”，心里像被针扎了一下。',
          gains: [{ type: '心情', value: -15 }, { type: '知名度', value: 500 }]
        },
        {
          text: '主导项目，保留“心法魂”',
          content: '你亲自坐在动捕设备前，一遍遍演示“虎鹤双形拳”的以刚攻柔和以弱反强：“不是单仿象模形，而是要以取神意为窍。”要求VR体验必须加入“师父语音彩蛋”——当用户动作不标准时，会响起你模仿师父的声音：“站如松，不是站如弹簧。”上线后，有大学生留言：“戴上VR练马步，听见那句‘气沉丹田’，突然想起爷爷教我种地时说的‘根要扎深’。”',
          gains: [{ type: '知名度', value: 1000000 }, { type: '武德', value: 1 }, { type: '声望', value: 5 }, { type: '资金', value: 30000 }, { type: '传承度', value: 10 }]
        }
      ]
    },
    {
      age: 40,
      content: '师父去世三年后，武馆收到了拆迁通知书——城市规划要建“岭南文化街”，武馆恰在红线内，开发商承诺补偿500万，或在文化街内保留50平米“展示区”。师兄们拿着通知书在师父灵前哭：“这是要断了洪家拳的根啊！”你摸着师父留下的铜烟杆（当年他用这杆敲你膝盖），烟杆上刻着“武馆是地，人是苗”。',
      condition: () => gameState.branches.isStayInGuan,
      choices: [
        {
          text: '死守老馆，集体抗议',
          content: () => {
            if (gameState.stats.reputation > 30) {
              return '你带着学员在武馆门口拉横幅，老邻居送来热水，退休警察张叔帮你们挡拆迁队：“这是佛山的根，不能挖！”僵持三个月后，规划微调，武馆得以保留，但外墙被削去一角，演武场小了一半。夜里你摸着残缺的墙，听见隔壁工地的打桩声，突然明白师父说的“地会老，苗要新”——武馆保住了，可学员们练拳时总被噪音打断。';
            } else {
              return '你来到开发商办公室抗议，但由于你的声望过低，没有什么人与你一道抗议，且开发商欺负你名声不大，驳回了你的抗议。你得到了开发商赔偿的金钱，但你们的武馆就这样被拆除了。';
            }
          },
          gains: () => {
            if (gameState.stats.reputation > 30) {
              return [{ type: '心情', value: -15 }, { type: '资金', value: -30000 }];
            } else {
              return [{ type: '资金', value: 500000 }, { type: '心情', value: -20 }, { type: '声望', value: -3 }, { type: '知名度', value: -200000 }];
            }
          }
        },
        {
          text: '接受拆迁，郊区重建',
          content: '你用补偿款在城郊租下废弃小学，带着学员们搬砖、刷墙，把老馆的青砖、木人桩、甚至院角那棵老槐树都移了过去。新武馆门口挂着木牌：“此馆所有砖瓦，皆来自佛山老武馆”。开馆那天，老学员从市区坐两小时公交来上课，有人说：“路远，但闻着这木头桩的味儿，就像回家了。”',
          gains: [{ type: '资金', value: 250000 }, { type: '知名度', value: -30000 }]
        },
        {
          text: '合作开发，文化街设“活态分馆”',
          content: '你和开发商谈判：武馆主馆迁到郊区，文化街内保留原武馆核心区域（师父的茶桌、你的马步桩、祖师爷画像），作为“洪家拳活态博物馆”，每周六由你带学员现场表演。新馆开业时，文化街的游客挤爆了分馆，有个小女孩摸着你当年扎马步的青砖：“阿姨说这上面有师父的汗味儿，是真的吗？”你笑着把她抱上桩：“你站一会儿，就知道了——这不是汗味儿，是‘劲儿’的味儿。”',
          gains: [{ type: '知名度', value: 100000 }, { type: '资金', value: 200000 }, { type: '声望', value: 3 }, { type: '传承度', value: 5 }]
        }
      ]
    },
    {
      age: 50,
      content: '武馆的年轻学员越来越多，但家长们开始抱怨：“孩子练三年还在学‘铁线拳’，隔壁武馆考黄绿带只要三个月！”年轻弟子阿杰（二十五岁，体育大学毕业）提出：“不如推出‘洪家拳速成班’，简化套路，突出‘实战防身’，对接武术考级。”老师兄拍桌子骂：“这是把祖宗拳改成‘广播体操’！”你望着墙上师父的照片，想起他说的“习武先习心，心不诚，拳再快也没用”。',
      condition: () => gameState.branches.isStayInGuan,
      choices: [
        {
          text: '拒绝简化，坚守传统',
          content: '你在馆内贴出告示：“振威武馆只教‘真拳’，不学速成，不考花带。”家长们纷纷转馆，学员从150人降到80人，但留下的都是真心爱拳的。三年后，这批孩子在“全国传统武术锦标赛”包揽少年组前三，评委说：“这拳里有‘老味儿’，是磨出来的。”',
          gains: [{ type: '知名度', value: -500 }, { type: '声望', value: 3 }]
        },
        {
          text: '推出“双轨制”：传统班与速成班',
          content: '你把武馆分成东西两区——东区教传统套路，从马步到拳谱，三年学完“铁线拳”；西区教速成班，三个月学“防身十三式”，对接考级。但某天你发现速成班的孩子把“工字虎伏拳”说成“工字虎俘拳”，传统班的孩子嘲笑他们“花架子”，两个班在演武场打了起来。你罚他们一起扎马步，说：“拳没有高低之分，心有。速成班的要记住，你的‘虎俘’，是老祖宗练了十年的‘伏虎’才能做到的；传统班的要知道，若没人学，别说‘虎俘’，连‘伏虎’都做不到。”',
          gains: [{ type: '声望', value: 2 }, { type: '知名度', value: 500 }, { type: '传统纯度', value: -3 }]
        },
        {
          text: '改良“传统班”，保留“魂”简化“形”',
          content: '你带着阿杰和老师兄一起研究：保留“工字伏虎拳、虎鹤双形拳、铁线拳”核心，将套路时长从20分钟压缩到8分钟，每招加入“心法口诀”（如“工字连环如伏虎，气贯丹田势吞云”）。新套路教给孩子们时，他们边打边念口诀，像唱儿歌。有家长说：“以前孩子背课文费劲，现在口诀倒背如流，马步也扎稳了。”',
          gains: [{ type: '传承度', value: 5 }, { type: '招生', value: 30 }]
        }
      ]
    },
    {
      age: 70,
      content: '你的膝盖已经弯不了90度，但每天清晨仍会坐在演武场边，看阿杰教孩子们“马步接龙”每人扎10秒，接力传递师父留下的铜烟杆。文化街的分馆里，你的VR教学视频成了“镇馆之宝”，有海外游客说：“我在法国的博物馆见过中国武术，但这里的拳，会‘呼吸’。”',
      condition: () => gameState.branches.isStayInGuan
    },
    {
      age: 70,
      content: `临终前，你把所有弟子叫到一起，让他们在你面前打一套改良后的“虎鹤双形拳”。孩子们打完，你指着最小的那个女孩：“把烟杆给她。”女孩才八岁，扎着羊角辫，是文化街分馆游客的孩子，当初因为摸青砖不肯走，被妈妈送来学拳。你摸着她的头：“记住，这烟杆敲过我的膝盖，也敲过你师父的膝盖——它敲的不是疼，是‘敬’，敬祖宗，敬功夫，敬自己这身骨头。”

你走时，手里攥着那半块七岁偷师时师父用的药酒棉。武馆的青砖墙上，新添了你的照片——七十岁的你，坐在演武场边，看着孩子们打拳，笑得像个孩子。`,
      condition: () => gameState.branches.isStayInGuan,
      choices: [
        {
          text: '游戏结束',
          content: '谢谢你体验这个关于武术传承的故事。',
          gains: []
        }
      ]
    }
  ]
})

// 藏书阁内容
const libraryContent = {
  mainOptions: [
    {
      id: 1,
      type: '了解武德',
      description: '学习武术道德规范和精神内涵',
      actionPoints: 1
    },
    {
      id: 2,
      type: '参悟武技',
      description: '深入研究武术招式和技巧',
      actionPoints: 1
    },
    {
      id: 3,
      type: '了解武魂',
      description: '探索武术精神和灵魂',
      actionPoints: 1
    }
  ],
  // 武术流派内容
  styles: {
    '洪拳': {
      wude: '你打开洪拳珍藏秘籍，映入眼前的就是洪拳的战术思想——以防为主，攻防交织，挑劈护中，闪穿封截，正面突破，长短结合，连环进击，偏门巧入，以刚为主，刚柔相济。',
      wuhun: {
        representatives: [
          {
            name: '洪熙官',
            description: '广州人，传为洪拳创始人。1673年，因清廷追捕，匿身广州大佛寺习武。值"三藩之乱"爆发，洪熙官借机广纳志士，建立反清武装。后与方世玉等设基地于肇庆鼎湖山庆云寺，秘密授武。因叛徒出卖，清兵围剿，洪熙官突围后隐姓埋名，融会百家武艺，创编洪拳，游走四方传授，自成体系。'
          },
          {
            name: '黄麒英',
            description: '南海西樵人，清末"广东十虎"之一。少时街头卖艺，为陆阿采所识，收为弟子，学艺十年，得其精髓。曾任镇粤将军兵技击教练，后设生草药店以补家用，艺成后开设宝芝林授徒。以虎鹤双形拳著称，将所学传予子黄飞鸿。'
          },
          {
            name: '梁坤（铁桥三）',
            description: '广东南海人（1813–1886）。幼嗜武，拜福建莆田少林觉因和尚为师，在广州白云山能仁寺带发修行七年，尽得真传。后受聘于富商蔡赞、洪熙官家族授武，育善堂施雨良、孙指添、区珠、林福成等皆其弟子。传下铁线拳，林福成再传黄飞鸿。'
          },
          {
            name: '林福成',
            description: '佛山人，铁桥三弟子。1871年，十五岁黄飞鸿救其于危难，为报恩，将铁线拳与飞砣绝技传授黄飞鸿，助其武艺大进。'
          },
          {
            name: '黄飞鸿',
            description: '黄飞鸿、原名黄锡祥，字达云，广东西樵人，岭南洪家拳宗师、名医。五岁随父黄麒英习武，后师从林福成学铁线拳，从宋辉镗习无影脚。1872年起在广州授武，曾任三栏行工人教练，1886年父逝后设宝芝林医馆。清末任"睇场"（保镖），1919年受邀于广州精武体育会表演。1924年广州商团暴动，宝芝林焚毁，黄飞鸿郁结成疾，1925年病逝于广州，享年六十九。身后萧条，赖友人资助安葬。妻莫桂兰携子徒移居香港授武。黄飞鸿绝技包括双飞铊、子母双刀、五郎八卦棍、无影脚、铁线拳、工字伏虎拳、虎鹤双形拳等，亦善舞狮，有"广州狮王"之称。医术精湛，尤擅驳骨疗伤，福军首领刘永福赠"技艺皆精"匾额，并聘为技击总教练，曾随军赴台抗倭。弟子有梁宽、林世荣、陈殿镖、凌云阶、邓芳、邓秀琼、莫桂兰等。'
          },
          {
            name: '林世荣',
            description: '林世荣，南海平洲人，绰号"猪肉荣"，师从黄飞鸿。清末广州武术竞赛中连胜数十人，获第一名，声名大振。热心公益，1921年以六旬高龄为孤儿院筹款，获孙中山颁发银质奖章。因与乐善戏院护卫李世桂争执，遭悬赏追捕，远走香港。后得弟子朱愚斋协助，刊印《工字伏虎拳》《虎鹤双形拳》《铁线拳》等拳谱，广泛传播洪拳。一生授徒逾万，为黄飞鸿弟子中成就最高者。弟子有赵教、刘湛、林祖、朱愚斋、邓二、关坤、谭就、孔纪南等。'
          }
        ],
        introduction: '洪家拳俗称洪拳，有300多年的发展历史，是广东"洪、刘、蔡、李、莫"五大名拳、十三家派之首。洪拳风格硬桥硬马，步稳势烈，刚柔并济，以气催力，发声助威，长桥重打，短桥逼封，手法丰富，技击性强为突出，是武术界公认为南拳中之精华。',
        tactics: '以防为主，攻防交织，挑劈护中，闪穿封截，正面突破，长短结合，连环进击，偏门巧入，以刚为主，刚柔相济',
        inheritance: '顺德洪拳的传承可追溯至清代，传承有序、谱系清晰。传承套路以洪拳三宝（工字伏虎拳、虎鹤双形拳、铁线拳）、三展铁线拳、洪拳精选等，器械以刀、五郎八卦棍（枪）春秋大刀、洪拳木人桩、桥手环、三星桩等为主。在顺德，洪拳主要为黄飞鸿、林世荣一脉。',
        treasures: [
          {
            name: '工字伏虎拳',
            description: '以步武进退成"工"字形，故名之。工字伏虎，腰马稳健，桥手刚劲，法门紧密，进退有规。恒久练习，不必站马而腰马自坚，不必打桩而桥手自劲。'
          },
          {
            name: '虎鹤双形拳',
            description: '虎先者，以刚攻柔，若猛虎下山、势不可挡；以力吃弱，若饿膚扑食，一击必杀。鹤协者，以弱反强，若鹤锐袭米，出奇制胜；以密破区，若鹤翅抖水，一碰击落。'
          },
          {
            name: '铁线拳',
            description: '硬如铁，柔似线，故名为之"铁线拳"。以气生力，逼硬桥手，由柔致刚，独门内功秘传也。以意运气，以气发声，以声催力，逼生硬功。'
          }
        ],
        culture: [
          {
            name: '醒狮',
            description: '洪拳和醒狮有着密不可分的关联，民间有谚——"只要世有祠堂，就有洪拳和醒狮"。在顺德地区，舞狮人多习有或精通洪拳，其舞狮动作多以洪拳马步为主。表演时，锣鼓擂响，舞狮人先打一阵洪拳，这称为"开桩"。'
          },
          {
            name: '洪拳龟苓膏',
            description: '由洪拳宗师黄飞鸿于1886年创制，经洪拳传人历代传承，保存至今。特点是清爽嫩滑、回甘不苦，采用老石金钱龟板、灵芝、土茯苓、鲜石斛、生地等多种名贵中草药熬制，具有滋阴补肾，润燥护肤等功效。现今，洪拳《龟苓膏制作技艺》列入非物质文化遗产名录。'
          }
        ]
      },
      skills: {
        '拳法': [
          {
            name: '工字伏虎拳',
            description: '你在瀑布下苦练工字伏虎拳，总觉刚猛有余却劲力难透。师父指向激流中磐石："水穿硬石，非靠瞬力，而凭绵长之劲。"你顿悟——洪拳的"铁桥硬马"需以柔劲为基，刚柔相济方能落地生根。再练时，腰马传力如潮涌，沙袋应声而破。（体魄+2，悟性+1，工字伏虎拳熟练度+12%）',
            gains: [
              { type: 'strength', value: 2 },
              { type: 'wisdom', value: 1 }
            ],
            proficiency: 12
          },
          {
            name: '铁线拳',
            description: '你苦练铁线拳，双臂套铁环，力求桥手硬如铁，却总感劲力僵滞。师父让你手捧一碗水练"老僧挑担"，水洒半分便重来。七日过去，你发现唯有肩松肘沉，以腰腹柔劲托举，水碗方能平稳。卸下铁环再打拳，臂如铁线般刚韧，拳锋破空竟隐带"吃"的催力之声，方悟铁线真意乃刚柔相济，非纯刚也。（悟性+2，体魄+2，铁线拳熟练度+15%）',
            gains: [
              { type: 'wisdom', value: 2 },
              { type: 'strength', value: 2 }
            ],
            proficiency: 15
          },
          {
            name: '虎鹤双形拳',
            description: '你苦练虎形刚猛，却总觉劲力难续。师父让你观察鹤鸟："鹤啄轻柔，却能穿木；虎啸雄浑，亦需调息。"你顿悟刚柔相生之理，将虎之威与鹤之灵融于一体，拳风顿如潮涌连绵。（悟性+2，体魄+1，虎鹤双形拳熟练度+14%）',
            gains: [
              { type: 'wisdom', value: 2 },
              { type: 'strength', value: 1 }
            ],
            proficiency: 14
          }
        ],
        '腿法': [
          {
            name: '滑步前脚锁步',
            description: '你在青石板上反复练习滑步锁足，却总被师父以轻巧的退步化解。师父以竹枝点地："水银泻地，非靠猛力，而在贴地而行、落步生根。"你顿悟锁步精髓在于"黏"而非"撞"，再练时步如流沙渗入，竟将师父逼得连退三步。（悟性+1，体魄+2，锁步熟练度+10%）',
            gains: [
              { type: 'wisdom', value: 1 },
              { type: 'strength', value: 2 }
            ],
            proficiency: 10
          },
          {
            name: '上步后脚锁步',
            description: '你苦练后脚锁步技法，却因重心不稳屡屡失手。师父让你单足立于梅花桩上，手持盛满墨汁的砚台练"灵鹤旋膝"，墨洒半滴便重来。三日后，你发现唯有以腰为轴、膝如缠丝，方能劲透足尖。再锁敌踝时，竟如铁箍扣木，令对手动弹不得。（体魄+2，悟性+1，锁步熟练度+12%）',
            gains: [
              { type: 'strength', value: 2 },
              { type: 'wisdom', value: 1 }
            ],
            proficiency: 12
          },
          {
            name: '锁步别摔',
            description: '你与木人桩对练锁步别摔，总觉手脚发力相斥。师父突然推来一个旋转的陀螺："顺其势而破其轴，方为真锁。"你恍然悟出锁步与拧腰需如齿轮咬合，再试时一招"老藤缠树"，竟将二百斤的师兄凌空摔出半丈。（体魄+2，悟性+1，锁步熟练度+15%）',
            gains: [
              { type: 'strength', value: 2 },
              { type: 'wisdom', value: 1 }
            ],
            proficiency: 15
          }
        ],
        '武器': [
          {
            name: '五郎八卦棍',
            description: '你在武馆空地演练五郎八卦棍法，虽记熟六十四点变化，却总觉招式流转生硬。师父以棍尖在沙地上划出太极八卦图："卦象相推非定死，阴阳互济方为真。"你瞥见竹叶随风绕枝旋落，忽悟棍法亦需如卦象流转——"先击四正，后击四隅" 不仅是方位顺序，更是阴阳劲力相生的节奏。再舞棍时，强攻乾位（正刚），借而后闪至巽位偏门（奇柔），刚柔之力如两仪轮转，竟引得周身竹叶随棍风成漩涡涌动。（体魄+3，悟性+2，五郎八卦棍熟练度+18%）',
            gains: [
              { type: 'strength', value: 3 },
              { type: 'wisdom', value: 2 }
            ],
            proficiency: 18
          },
          {
            name: '护环短刀',
            description: '你初练双刀时总追求招式繁复，师父以刀环轻叩你手背："刀势如潮，环护神魂。"你闭目凝神，双刀分合间护环嗡鸣如浪涌，忽觉刀锋与呼吸同频。当长枪横扫下盘，你旋身踏缠丝步，双刀交错如剪，护环锁敌刃，反手刀尖已点中其腕。原来"猛中藏巧，环破长兵"。（体魄+3，护环短刀熟练度+18%）',
            gains: [
              { type: 'strength', value: 3 }
            ],
            proficiency: 18
          }
        ]
      }
    },
    '蔡李佛拳': {
      wude: '你看着贴在墙上的蔡李佛拳海报，海报上画着祖师爷的画像，下面写着"进可御外辱，退可强身健魄"，你深受感染。',
      wuhun: {
        representatives: [
          {
            name: '陈亨',
            description: '陈享自幼习武，7岁随族叔陈远护学佛家拳；15岁拜李友山为师学李家拳；后又师从隐居罗浮山的少林还俗高僧蔡福学蔡家拳近十年。1836年，陈享集三家拳术之长创编出蔡李佛拳，并以三位恩师的姓氏命名，表达尊师重道之意陈享一生不仅开馆授徒、广传武艺，还曾协助林则徐训练水师抗英，参与太平天国起义等历史事件他晚年总结武学经验，编撰拳谱，使蔡李佛拳形成完整体系，成为岭南武术的重要代表。'
          },
          {
            name: '张炎（鸿胜）',
            description: '张炎，蔡李佛拳鸿胜馆始祖，1824年－1893年，祖籍新会双水东凌村。张炎是金山大埠华侨商人之子，在陈享打败基利士之后，华侨十分振奋，他父亲立即送他向陈享拜师学艺。此后，他从海外一直追随到国内，他还以陈享的武馆为名，改名张鸿胜。他师从陈享及蔡福，1851年在佛山创立鸿胜馆，将蔡李佛拳传播至海内外，门人遍布五大洲四十多个国家。'
          },
          {
            name: '夏碧慈',
            description: '夏碧慈是一位在省港澳很有名气的武术家和伤科医生。他的拿手武艺是蔡李佛的醉八仙拳。夏碧慈有女儿夏剑萍继承了他的衣钵。夏剑萍师傅曾在香港九龙开馆授徒多年。门徒甚多，遍港澳。她的跌打伤科在港非常的有名气。她退休了以后就移民到加拿大去定居了。'
          },
          {
            name: '郭天扬',
            description: '郭天扬，在广州武林中人都叫他做"外江扬"。他的蔡李佛九节鞭练得出神入化。到了晚年，他常用一条毛巾放在脖子上。要是遇到了有人要攻击他，他就马上把毛巾卷起来当做鞭子用来防身。他的继承人乃是他的儿子郭贵波、郭桂添以及郭桂南。郭桂添现在广东清远开馆授徒与行医济世。桃李满门。'
          }
        ],
        introduction: '蔡李佛拳的武术体系博大精深，涵盖了拳术、对拆、器械、桩功等多个方面。拳术套路多达39套，对拆类54套，器械类包含64套（其中棍术14套），桩类练习法18套，狮艺套路9套，以及内功练习套路等，总计达193套之多。在技击手法上，拥有30种手法，28种掌法，29种桥法，35种槌（拳）法，同时还有14种身法，16种腿法，18种步法，构成了一套完整且多样的武术体系。',
        tactics: '步稳架大，势雄力猛，左右开弓，长短桥并用，以左掌为防，右拳为攻，攻防严谨，手法多变，连环击打。基本动作包括穿、捞、挂、扫、插，拳法有直冲、横冲、直扫、横扫、斜扫，掌法有铲掌、抛托等，桥法有沉桥、截桥、缠桥等。',
        inheritance: '蔡李佛拳自创立以来，通过张炎（鸿胜）等弟子的努力，在佛山创立鸿胜馆，将拳术传播至海内外，门人遍布五大洲四十多个国家。历代传人都致力于传承和弘扬这一拳种，使其成为岭南武术的重要组成部分。演练中注重发声配合动作，以"域""的""益""吓""鹤"五音为标式，尤以"益、的、域"三音为流派识别标志。',
        treasures: [
          {
            name: '挂',
            description: '蔡李佛拳中，挂字诀属抢攻性的攻击手法。"攻即化、化即攻"，"防之必然、攻之无意"是蔡李佛以攻为主、攻防兼备技法显著特点。挂字诀善于攻时疾速迅猛，守时封关砸节，多以配合其他技法运用，如"挂蟠捎"、"挂抢擂"、"挂擒抛"等散手技法。'
          },
          {
            name: '捎',
            description: '捎槌属攻击面最大，力度最强的手法。"捎"者广东四邑俗语，含有抱摔、砍削、扫击的意思。分有从上而下的"企捎"、如现代散打摆拳的"横捎"和从下而上的"楠捎"等技法。'
          },
          {
            name: '插',
            description: '蔡李佛之插槌，握法是先把五指平伸，然後把四指屈曲至第二节骨为止，姆指紧扣在食指之旁，掌心微微内含成窝状，俗称"姜子槌"。分有"正插、阳插、阴插、弹插、侧插"等五种，是蔡李佛常见的攻击技法。'
          }
        ],
        tenTechniques: [
          { name: '穿', description: '吊马双穿手' },
          { name: '挠', description: '缠丝马辘桥' },
          { name: '钑', description: '左挂右钑槌' },
          { name: '抛', description: '醉仙敬酒' },
          { name: '标', description: '迈马标撞槌' },
          { name: '顶', description: '上马攻桥' },
          { name: '撞', description: '扭马反撞槌' },
          { name: '挂', description: '挂捎带打' },
          { name: '捎', description: '企捎、横捎、楠捎' },
          { name: '插', description: '正插、阳插、阴插、弹插、侧插' }
        ],
        culture: [
          {
            name: '爱国精神',
            description: '蔡李佛拳创始人陈享曾协助林则徐训练水师抗英，参与太平天国起义等历史事件，体现了武术与爱国精神的紧密结合。'
          },
          {
            name: '伤科医术',
            description: '许多蔡李佛拳传人同时精通伤科医术，如夏碧慈、夏剑萍父女，将武术与中医结合，形成了独特的武术医疗文化。'
          }
        ]
      },
      skills: {
        '拳法': [
          {
            name: '醉仙敬酒',
            description: '师兄演示醉仙敬酒时步法飘忽，你屡次失衡。他递来一碗水："水满不洒，虚实时变。"你仿醉汉踉跄，重心藏于缠丝马辘桥中，水碗稳如磐石。霎时领悟"虚则实之，实则虚之"之理，上马攻桥如风卷云涌。（悟性+2，体魄+1，醉仙敬酒熟练度+15%）',
            gains: [
              { type: 'wisdom', value: 2 },
              { type: 'strength', value: 1 }
            ],
            proficiency: 15
          },
          {
            name: '缠丝马辘桥',
            description: '师姐演示"缠丝马辘桥"时，步如辘轳旋转，手法缠绕难测。你模仿其形，却屡被轻易带偏。师姐让你闭眼，仅凭手臂相搭感知其劲力流动。初时只觉混沌，渐渐指尖捕捉到一丝规律：其劲如丝缠绕，虚实时变。你顺势借力，步法自然随之拧转，如马辘滚动，霎时领悟"有桥桥上过，无桥问有桥"之妙。（悟性+2，体魄+1，缠丝马辘桥熟练度+14%）',
            gains: [
              { type: 'wisdom', value: 2 },
              { type: 'strength', value: 1 }
            ],
            proficiency: 14
          },
          {
            name: '上马攻桥',
            description: '师兄演示"上马攻桥"时步如奔马，你却重心不稳。他让你持棍踏浪："浪涌不倾，借势发力。"你忽感腰马如辘轳旋动，桥手随步法冲撞，似巨浪拍岸。原来"上马"非蛮力，乃借地生劲。（悟性+1，体魄+3，上马攻桥熟练度+15%）',
            gains: [
              { type: 'wisdom', value: 1 },
              { type: 'strength', value: 3 }
            ],
            proficiency: 15
          }
        ],
        '腿法': [
          {
            name: '单飞脚',
            description: '你初练单飞脚，腾空失衡，击拍无力。师姐点醒："腾空非蛮跳，乃腰脊如弓！"你凝神引气上提，蹬地如箭离弦，空中右腿绷直弹踢，手击脚面清脆响亮，左腿屈膝收控。落地时顿悟：助跑击步化水平速度为腾空高度，方得"步如风火轮，身似云中鹤"之妙。（体魄+2，悟性+1，单飞脚熟练度+16%）',
            gains: [
              { type: 'strength', value: 2 },
              { type: 'wisdom', value: 1 }
            ],
            proficiency: 16
          },
          {
            name: '走生马',
            description: '你初练步法时总求迅疾，师父以棍梢轻点你膝窝："马步为根，活步如云。"你闭目沉腰，八步连环如织网，忽觉地面颤动似蛇行。当师弟连环踢扫下盘，你偷马转骑龙，缠丝步贴地旋绕，肘底锤已轻触其肋。原来"定步坐钟，活步浮云"。（体魄+3，走生马熟练度+18%）',
            gains: [
              { type: 'strength', value: 3 }
            ],
            proficiency: 18
          }
        ],
        '武器': [
          {
            name: '蝴蝶双刀',
            description: '你初练双刀时总贪求凌厉，师父以竹枝轻点你肘尖："刀如蝶翼，步似穿花。"你闭目沉腕，双刀在摊、耕、拦、滚间流转，忽觉刀锋与呼吸共震。双刀直刺心口，你侧身躲避，刀尖滚入对方腕隙，一枕一压如蝶吻荆棘。原来"攻藏于守，步引刀发"。（悟性+3，体魄+2，蝴蝶双刀熟练度+15%）',
            gains: [
              { type: 'wisdom', value: 3 },
              { type: 'strength', value: 2 }
            ],
            proficiency: 15
          },
          {
            name: '十字梅花双刀',
            description: '师姐手持双刀立于桩前，刀尖点地如梅枝含露。你依样起势，却觉双刀互绊，步法滞涩。师姐轻叩你腕门："双刀看走，步步生梅！十字非交叉形，而是左右交替、进退闪迫之机。"你闭目沉心，唯闻刀风飒飒如落梅拂过——初时混沌，渐觉师姐刀势如织网：旋刀为瓣，兜刀为蕊，每一步皆踏梅花五方位。（悟性+3，体魄+1，十字梅花双刀熟练度+12%）',
            gains: [
              { type: 'wisdom', value: 3 },
              { type: 'strength', value: 1 }
            ],
            proficiency: 12
          }
        ]
      }
    },
    '咏春': {
      wude: '你看着大堂中央裱着的咏春祖训——"勤练习技不离身，养正气戒滥纷争，当处世态度温文，扶弱小以武辅仁。"，这句话一直萦绕在你的脑海中，深受启发。',
      wuhun: {
        representatives: [
          {
            name: '叶问',
            description: '叶问（1893年10月1日—1972年12月1日），祖籍广东省南海县桑园，是当地大少爷，在香港圣士提反学院读书，学识渊博。咏春师承陈华顺、梁璧，为咏春拳体系的开宗立派人。叶问于50年代开始在香港教授广东人咏春拳；其封门弟子梁挺将咏春拳传扬国际，更通过其子弟在全球的广泛传扬，形成出一套最权威的中国传统武术实战修习课程《梁挺咏春》，载誉全球。叶问是咏春拳乃至中国武术一致推崇的一代宗师，叶问众弟子当中就有青出于蓝的，其中除了封门弟子梁挺外，还有将中国武术闻名世界的李小龙。'
          },
          {
            name: '梁赞',
            description: '梁赞（赞先生），自随华宝习咏春以后，他即感到咏春拳在法度用力、身型和手法上，无一不是上乘之法。梁赞约于1870~1890年在赞生堂内收徒授拳，将其毕生所学重新整理。然而，梁赞并不公开授徒，始终以行医为业，因店务缠身，他只收了几个关门弟子，未能广授徒众，所以并未令咏春拳盛极一时。能得其真传者，除其二子梁春及梁璧外，仅陈华顺一人而已。梁赞六十多岁后，两个儿子梁璧、梁春皆有工作或已经离开佛山，不能接替父亲的生意，唯有将赞生堂转让给他人并改名杏济堂。梁赞退休后返回古劳镇。'
          },
          {
            name: '陈华顺',
            description: '陈华顺（华公），因找钱事业，常经过赞生堂，由于他知道佛山梁赞的比武事迹，时常从门缝里偷看自学。终于在39岁时追随已经62岁的梁赞学习咏春拳。梁赞去世后，陈华顺（于1901后）于莲花地大街缸瓦店内教授咏春拳术。然而咏春授拳之法与一般少林拳术不同，因它需要通过长期过手之练习，而过手之最佳练法，需由个别教授，故未容多教，因此陈华顺收费颇昂，遂未为一般人士所能负担，而能学者，多为贵家公子，当时有"少爷拳"之称号，故未能广泛流传。陈华顺于1901~1907年间授拳，传人共有16人。而能得陈华顺之技者，有吴仲素、何汉侣、雷汝济、其子陈汝棉及封门弟子叶问。'
          },
          {
            name: '李小龙',
            description: '李小龙幼年因体弱而拜名师叶问习咏春拳术，18岁往美国留学，毕业后在美国西雅图开设"振藩国术馆"传授中国武术。期间他悉心研究、吸收中外技击精华，创截拳道。1971年返港从事影视业工作，拍摄了多部轰动世界影坛的中国功夫片，其本人被誉为"功夫之王"，他早年所习的佛山咏春拳也因此在世界各地大受欢迎。'
          }
        ],
        introduction: '咏春拳相传由少林五枚师太创始，传于严咏春，故名「咏春拳」。后经梁赞(佛山赞先生)整理发扬，再由叶问带到香港并广传世界，成为现代最知名的中国武术之一。它是一种集内家拳法和近打于一身的拳术，要求手、腰、马、心、意、劲整体合一。强调以"心"指挥"意"，以意引导手、腰、马运动，从而形成整体合一。它立足于实战，具有招式多变、运用灵活、出拳弹性、短桥窄马、擅发寸劲的主要特点，以大闪侧、小俯仰、耕拦摊膀、黏摸荡捋、审势记牢、曲手留中为手法，以搭、截、沉、标、膀、腕指、黏、摸、熨荡、偷、漏和"二字钳羊马"的身形步法为标志。基本手法以三傍手为主，还有挫手、撩手、破排手、沉桥、粘打。主要步型有四平马、三字马、追马、跪马、独立步等。',
        tactics: '（1）中线理论：拳谚云「两点之间，直线最短」，所有攻防都围绕人体中线（由头至胯的虚拟直线）进行，效率最高。\n（2）朝面追形：始终正面对着对手，控制其重心线。\n（3）短桥窄马：二字钳羊马步窄小，便于转向和卸力；手法活动范围短，利于近身爆发。',
        inheritance: '咏春拳传承谱系清晰，从五枚师太传于严咏春，再经梁赞整理发扬，陈华顺传承，最后由叶问带到香港并广传世界。叶问在香港传授咏春拳，培养了众多弟子，其中李小龙将咏春拳的理念融入截拳道，并通过电影将中国武术推向世界。如今，咏春拳已在全球范围内广泛传播。',
        treasures: [
          {
            name: '小念头',
            description: '入门套路，是「念头」或「意念」的训练，包含了咏春大部分基本手法。'
          },
          {
            name: '寻桥',
            description: '中级套路，训练步法与手法的配合，以及「追形」的能力。'
          },
          {
            name: '标指',
            description: '高级套路，手法较为狠辣，用于反击和破解擒拿，以及应付多人攻击。'
          }
        ],
        trainingMethods: [
          {
            name: '木人桩',
            description: '咏春标志，用于练习手法、角度、距离感和劲力。'
          },
          {
            name: '黐手',
            description: '核心训练，通过双手臂的接触来感知对方意图(听劲)，并作出条件反射式的攻防。'
          }
        ],
        culture: [
          {
            name: '咏春祖训',
            description: '"勤练习技不离身，养正气戒滥纷争，当处世态度温文，扶弱小以武辅仁。"这一祖训体现了咏春拳不仅注重武技的修炼，更强调武德的培养。'
          },
          {
            name: '电影文化',
            description: '通过李小龙、叶问等电影作品，咏春拳已成为中国武术文化的重要符号，在全球范围内产生了深远影响，促进了中国传统文化的国际传播。'
          }
        ]
      },
      skills: {
        '拳法': [
          { name: '小念头',
            description: '你练小念头时总贪快，师父以竹枝轻点你手腕："念在拳先，形随意动。"你闭眼慢打，呼吸与摊手伏手同频，忽觉指尖气流涌动。当敌拳突至，你本能以寻桥步法侧身，标指已抵其喉。原来"慢练求意，意到劲发"。（悟性+2，小念头熟练度+10%）',
            gains: [
              { type: 'wisdom', value: 2 }
            ],
            proficiency: 10
          },
          { name: '寻桥',
            description: '你与师弟黐手练习"寻桥"，总急于发力突破，反被屡屡制住中线。师父按住你双桥，令你静心感知其劲力方向。你放弃抵抗，臂如藤蔓贴附，忽觉对方力道微偏，瞬间借势切入，一指直指其喉。师父点头道："寻桥非攻桥，是寻其破绽，后发先至。"你方明"寻"之真谛在于触觉听劲，非肉眼追逐。（体魄+2，悟性+2，寻桥熟练度+16%）',
            gains: [
              { type: 'strength', value: 2 },
              { type: 'wisdom', value: 2 }
            ],
            proficiency: 16
          },
          { name: '标指',
            description: '练标指时急于取眼，反被师弟轻易化解。师父以竹枝挡你视线："勿凝于指，而忘其月。"你闭眼沉心，指尖如藤条般轻柔探出，忽觉敌劲微偏，瞬间标指已抵其喉。方悟"败形中寻机，后发先至"。（悟性+2，体魄+1，标指熟练度+10%）',
            gains: [
              { type: 'wisdom', value: 2 },
              { type: 'strength', value: 1 }
            ],
            proficiency: 10
          }
        ],
        '腿法': [
          { name: '内钳阳马',
            description: '你初次站成"不丁不八"之状，刻意将膝关节向内旋钳，却觉髋部僵硬，重心虚浮。师兄以掌压你胯骨道："钳膝非死力，乃腰胯互拉之活劲！"你闭目沉心，忽觉两膝如磁相吸，髋关节前后对拉——前脚三分力轻灵探敌，后脚七分力稳扎如根。身形含胸收腹间，竟自然形成攻守兼备之三角架。霎时明悟"侧身藏中，钳阳护裆"之妙：此马既能膨胀外部肌肉发力，又可借膝内钳力守护下盘，实为咏春近身格斗之核心。（体魄+2，悟性+1，内钳阳马熟练度+15%）',
            gains: [
              { type: 'strength', value: 2 },
              { type: 'wisdom', value: 1 }
            ],
            proficiency: 15
          },
          { name: '侧撑腿',
            description: '你初次模仿侧撑腿，刻意保持头肩正平、支撑脚内扣，却觉腰胯僵滞，踢腿软绵。师姐以掌轻压你膝窝："劲从地起，贯胯而发！"你闭目凝神，忽忆标指训练中的劲力流转，再起腿时放松踝部，腰胯如磨盘外旋，脚掌斜切而出，木桩闷响。霎时明悟"撑"劲真意——不起高腿，以斜直线破空，反作用力沉于地，实战可截击膝胯。（悟性+2，体魄+1，侧撑腿熟练度+15%）',
            gains: [
              { type: 'wisdom', value: 2 },
              { type: 'strength', value: 1 }
            ],
            proficiency: 15
          }
        ],
        '武器': [
          { name: '八斩刀',
            description: '你握刀时总想强攻，师父以刀背压住你手腕："走位为魂，刀走人先。"你闭眼听风，步法如蝶穿花，忽觉刀尖已贴敌腕。当长枪劈面而来，你旋身错步，双刀绞缠如剪，反手刀尖点中其肘。原来"避走为攻，寸短寸险"。（悟性+1，体魄＋2，八斩刀熟练度+15%）',
            gains: [
              { type: 'wisdom', value: 1 },
              { type: 'strength', value: 2 }
            ],
            proficiency: 15
          },
          { name: '六点半棍',
            description: '你运棍时总求刚猛，师父以棍梢轻压你肘尖："棍是臂延，劲透梢尖。"你闭目感棍，四平马稳如磐石，忽觉棍头微颤如探水。当长枪横扫下盘，你偏身转马，棍尖弹抖如蛇，一钉直刺对方腕骨。原来"长桥发力，棍无二响"。（悟性+2，体魄+3，六点半棍熟练度+20%）',
            gains: [
              { type: 'wisdom', value: 2 },
              { type: 'strength', value: 3 }
            ],
            proficiency: 20
          }
        ]
      }
    }
  },
  // 随机事件
  randomEvents: [
    {
      id: 1,
      name: '前辈的墨迹',
      description: '在一卷秘籍的夹页中，发现前辈留下的批注，但字迹潦草难辨。',
      choices: [
        {
          name: '仔细揣摩批注含义',
          result: '前辈的批注对你而言大有裨益，你参悟了秘籍的奥妙',
          gains: [
            { type: 'wisdom', value: 2 }
          ]
        },
        {
          name: '忽略批注，按自己的理解练习',
          result: '你理解了半天都难以参悟其中奥妙',
          gains: [
            { type: 'wisdom', value: -1 },
            { type: 'mood', value: -1 }
          ]
        }
      ]
    },
    {
      id: 2,
      name: '贪多嚼不烂',
      description: '你同时研究蔡李佛拳与咏春秘籍，感觉招式要领开始混淆。',
      choices: [
        {
          name: '专注攻克一个',
          result: '你对该门派的武术套路理解加深',
          gains: [
            { type: 'wisdom', value: 2 }
          ]
        },
        {
          name: '仍然坚持同时研究',
          result: '你两边都没有研究出什么东西',
          gains: [
            { type: 'mood', value: -2 },
            { type: 'wisdom', value: -1 }
          ]
        }
      ]
    },
    {
      id: 3,
      name: '守护者的试炼',
      description: '藏书阁长老见你勤勉，提出用三招考验你对《洪拳》残卷的理解。',
      choices: [
        {
          name: '接受挑战',
          result: '你演示自己所学，得到了长老的额外指点',
          gains: [
            { type: 'wisdom', value: 2 }
          ]
        },
        {
          name: '婉拒',
          result: '你认为火候未到，拒绝了长老的提议',
          gains: [
            { type: 'mood', value: -1 }
          ]
        }
      ]
    }
  ]
}

// 获取当前年份的故事
const getCurrentStory = () => {
  // 获取当前年份所有符合条件的故事
  const eligibleStories = character.lifeStory.filter(story => {
    return story.age === gameState.currentAge && 
           (!story.condition || (typeof story.condition === 'function' && story.condition()))
  });
  // 返回当前索引对应的故事
  return eligibleStories[gameState.currentStoryIndex] || null;
}

// 获取属性对应的中文名称
const getAttributeName = (type) => {
  const names = {
    strength: '体魄',
    martialArts: '武德',
    personality: '人品',
    mood: '心情',
    perseverance: '恒心',
    reputation: '声望',
    funds: '资金',
    inheritance: '传承度',
    traditionPurity: '传统纯度',
    governmentAttention: '政府关注度',
    network: '人脉',
    admissionCount: '招生人数',
    popularity: '知名度',
    wordOfMouth: '口碑'
  }
  return names[type] || type
}

// 应用属性变化
const applyAttributeChanges = (changes) => {
  if (!changes || !Array.isArray(changes)) return
  
  changes.forEach(change => {
    switch(change.type) {
      case 'strength':
        strengthValue.value += change.value
        if (strengthValue.value < 0) strengthValue.value = 0
        break
      case 'martialArts':
        martialArtsValue.value += change.value
        if (martialArtsValue.value < 0) martialArtsValue.value = 0
        break
      case 'personality':
        personalityValue.value += change.value
        if (personalityValue.value < 0) personalityValue.value = 0
        break
      case 'mood':
        moodValue.value += change.value
        if (moodValue.value < 0) moodValue.value = 0
        break
      case 'perseverance':
        perseveranceValue.value += change.value
        if (perseveranceValue.value < 0) perseveranceValue.value = 0
        break
      case 'reputation':
        reputationValue.value += change.value
        if (reputationValue.value < 0) reputationValue.value = 0
        break
      case 'funds':
        fundsValue.value += change.value
        if (fundsValue.value < 0) fundsValue.value = 0
        break
      case 'inheritance':
        inheritanceValue.value += change.value
        if (inheritanceValue.value < 0) inheritanceValue.value = 0
        else if (inheritanceValue.value > 100) inheritanceValue.value = 100
        break
      case 'traditionPurity':
        traditionPurityValue.value += change.value
        if (traditionPurityValue.value < 0) traditionPurityValue.value = 0
        else if (traditionPurityValue.value > 100) traditionPurityValue.value = 100
        break
      case 'governmentAttention':
        governmentAttentionValue.value += change.value
        if (governmentAttentionValue.value < 0) governmentAttentionValue.value = 0
        break
      case 'network':
        networkValue.value += change.value
        if (networkValue.value < 0) networkValue.value = 0
        break
    }
  })
}

// 应用关系变化
const applyRelationshipChanges = (changes) => {
  if (!changes || !Array.isArray(changes)) return
  
  changes.forEach(change => {
    if (gameState.relationships[change.person] !== undefined) {
      gameState.relationships[change.person] += change.change
      if (gameState.relationships[change.person] < 0) {
        gameState.relationships[change.person] = 0
      }
    }
  })
}

// 拜访角色函数
const visitCharacter = (characterName) => {
  // 检查是否有行动点
  if (actionPoints.value <= 0) {
    alert('行动点不足，无法拜访！')
    return
  }
  
  // 消耗行动点
  actionPoints.value--
  
  // 随机选择一个事件
  const characterEvents = relationshipEvents[characterName]
  const randomEvent = characterEvents[Math.floor(Math.random() * characterEvents.length)]
  
  // 显示事件选项
  gameState.currentEvent = {
    title: `拜访${characterName}`,
    description: randomEvent.name,
    choices: randomEvent.choices.map(choice => ({
      text: choice.text,
      action: () => {
        // 应用选择的效果
        if (choice.gains) {
          choice.gains.forEach(gain => {
            if (gain.type === 'relationship') {
              // 修改关系值
              gameState.relationships[gain.target] = (gameState.relationships[gain.target] || 0) + gain.value
              if (gameState.relationships[gain.target] < 0) {
                gameState.relationships[gain.target] = 0
              }
            } else {
              // 修改属性值
              applyAttributeChanges([gain])
            }
          })
        }
        
        // 记录事件
        gameState.history.push({
          year: gameState.currentAge,
          event: `${characterName} - ${randomEvent.name}: ${choice.description}`,
          gains: choice.gains
        })
        
        gameState.currentEvent = null
      }
    }))
  }
}

// 进入擂台比武
const enterArena = (arenaId) => {
  const arena = combatArena.arenas.find(a => a.id === arenaId)
  if (!arena) return
  
  // 检查是否有行动点
  if (actionPoints.value <= 0) {
    alert('行动点不足，无法进入擂台！')
    return
  }
  
  // 检查年龄限制
  if (gameState.currentAge < arena.requiredAge) {
    gameState.currentEvent = {
      title: '年龄限制',
      description: `对不起，你的年龄还不适合打该擂台，请长大以后再来。`,
      choices: [
        { text: '明白了', action: () => {
          gameState.currentEvent = null
        }}
      ]
    }
    return
  }
  
  // 消耗行动点
  actionPoints.value--
  
  // 显示选择武术流派界面
  gameState.currentEvent = {
    title: `${arena.name} - 选择武术流派`,
    description: `请选择你要使用的武术流派：`,
    choices: arena.styles.map(style => ({
      text: style.name,
      action: () => selectStyleForArena(arena, style)
    })).concat([{
      text: '取消',
      action: () => {
        actionPoints.value++ // 恢复行动点
        limitActionPoints() // 确保不超过上限
        gameState.currentEvent = null
      }
    }])
  }
}

// 选择流派后进行比武
const selectStyleForArena = (arena, style) => {
  // 模拟战斗结果（50%胜率）
  const isWin = Math.random() > 0.5
  const resultText = isWin ? '胜利' : '失败'
  const rewards = isWin ? arena.rewards.win : arena.rewards.lose
  
  // 学习技能（随机选择一个）
  const learnedSkill = style.skills[Math.floor(Math.random() * style.skills.length)]
  
  // 应用奖励/惩罚
  applyAttributeChanges(rewards)
  
  // 记录学习的技能
  if (isWin && !gameState.learnedSkills.includes(learnedSkill.id)) {
    gameState.learnedSkills.push(learnedSkill.id)
    // 更新技能熟练度
    updateSkillProficiency(learnedSkill.name, 20) // 初始熟练度20%
  }
  
  // 显示战斗结果
  gameState.currentEvent = {
    title: `${arena.name} - 战斗${resultText}`,
    description: isWin 
      ? `恭喜！你在${arena.name}中取得了胜利！${learnedSkill ? `你学会了新技能：${learnedSkill.name}` : ''}`
      : `很遗憾，你在${arena.name}中失败了。不要灰心，继续努力！`,
    details: rewards.map(gain => `${getAttributeName(gain.type)}${gain.value > 0 ? '+' : ''}${gain.value}`),
    choices: [
      { 
        text: '返回', 
        action: () => {
          // 记录事件
          gameState.history.push({
            year: gameState.currentAge,
            event: `${arena.name} - ${style.name} - ${resultText}${isWin && learnedSkill ? ` - 学会${learnedSkill.name}` : ''}`,
            gains: rewards
          })
          
          gameState.currentEvent = null
        }
      }
    ]
  }
}

// 获取可用任务
const getAvailableTasks = () => {
  let filteredTasks = availableTasks.filter(task => gameState.currentAge >= task.requiredAge)
  if (gameState.currentTaskType) {
    filteredTasks = filteredTasks.filter(task => task.type === gameState.currentTaskType)
  }
  return filteredTasks
}

// 切换任务面板显示
const toggleTasks = () => {
  gameState.showTasks = !gameState.showTasks
  gameState.taskResult = null
}

// 根据类型切换任务面板
const toggleTaskByType = (type) => {
  gameState.currentTaskType = type
  gameState.showTasks = true
  gameState.taskResult = null
}

// 选择任务
const selectTask = (task) => {
  // 检查是否有行动点
  if (actionPoints.value <= 0) {
    alert('行动点不足，无法选择任务！')
    return
  }
  gameState.currentTask = task
  gameState.selectedTaskChoice = null
  gameState.selectedSubtask = null
}

// 选择任务选项
const selectTaskChoice = (choice) => {
  gameState.selectedTaskChoice = choice
}

// 完成任务选项
const completeTaskChoice = () => {
  if (!gameState.currentTask || !gameState.selectedTaskChoice) return
  
  // 消耗一个行动点
  actionPoints.value -= 1
  
  // 应用选项增益
  const gains = gameState.selectedTaskChoice.gains
  if (gains && gains.length > 0) {
    applyAttributeChanges(gains)
  }
  
  gameState.taskResult = {
    success: true, 
    gains: gains,
    choiceResult: gameState.selectedTaskChoice.result,
    lifeLesson: gameState.currentTask.lifeLesson
  }
  
  // 重置当前任务
  gameState.currentTask = null
  gameState.selectedTaskChoice = null
}

// 选择子任务
const selectSubtask = (subtask) => {
  gameState.selectedSubtask = subtask
}

// 完成子任务
const completeSubtask = (option) => {
  if (!gameState.currentTask || !gameState.selectedSubtask || !option) return
  
  // 应用选项增益
  const gains = option.gains
  if (gains && gains.length > 0) {
    applyAttributeChanges(gains)
  }
  
  // 记录子任务结果
  if (!gameState.subtaskResults) {
    gameState.subtaskResults = []
  }
  gameState.subtaskResults.push({
    subtaskName: gameState.selectedSubtask.name,
    success: option.success,
    result: option.text
  })
  
  gameState.selectedSubtask = null
}

// 完成整个任务
const completeTask = () => {
  if (!gameState.currentTask) return
  
  // 检查是否有行动点
  if (actionPoints.value <= 0) {
    alert('行动点不足，无法完成任务！')
    return
  }
  
  // 特殊处理有选项的任务
  if (gameState.currentTask.choices) {
    if (!gameState.selectedTaskChoice) {
      alert('请选择一个选项！')
      return
    }
    return completeTaskChoice()
  }
  
  // 特殊处理有子任务的任务
  if (gameState.currentTask.subtasks) {
    // 这里可以添加子任务全部完成的检查逻辑
  }
  
  // 普通任务处理
  // 消耗一个行动点
  actionPoints.value -= 1
  
  // 应用任务增益
  const gains = gameState.currentTask.gains
  if (gains && gains.length > 0) {
    applyAttributeChanges(gains)
    gameState.taskResult = { success: true, gains: gains }
  }
  
  // 处理视频内容和测验
  if (gameState.currentTask.videoContent || gameState.currentTask.quiz) {
    // 这里可以添加视频播放和测验的逻辑
    console.log('视频内容:', gameState.currentTask.videoContent)
    console.log('测验内容:', gameState.currentTask.quiz)
  }
  
  // 处理小游戏
  if (gameState.currentTask.miniGame) {
    console.log('小游戏:', gameState.currentTask.miniGame)
    // 这里可以添加小游戏的逻辑
  }
  
  // 重置当前任务
  gameState.currentTask = null
  gameState.selectedSubtask = null
  gameState.subtaskResults = null
}

// 取消任务
const cancelTask = () => {
  gameState.currentTask = null
  gameState.taskResult = null
  gameState.selectedTaskChoice = null
  gameState.selectedSubtask = null
  gameState.subtaskResults = null
}

// 更新技能熟练度
const updateSkillProficiency = (skillName, value) => {
  if (gameState.skillProficiency[skillName] !== undefined) {
    gameState.skillProficiency[skillName] += value
    // 确保熟练度在0-100之间
    if (gameState.skillProficiency[skillName] < 0) {
      gameState.skillProficiency[skillName] = 0
    } else if (gameState.skillProficiency[skillName] > 100) {
      gameState.skillProficiency[skillName] = 100
    }
  }
}

// 切换藏书阁显示
const toggleLibrary = () => {
  gameState.showLibrary = !gameState.showLibrary
  gameState.libraryResult = null
}

// 选择藏书阁动作
const selectLibraryAction = (action) => {
  // 检查是否有行动点
  if (actionPoints.value <= 0) {
    alert('行动点不足，无法选择动作！')
    return
  }
  gameState.currentLibraryAction = action
}

// 完成藏书阁动作
const completeLibraryAction = () => {
  if (!gameState.currentLibraryAction) return
  
  // 检查是否有行动点
  if (actionPoints.value <= 0) {
    alert('行动点不足，无法完成动作！')
    return
  }
  
  // 消耗一个行动点
  actionPoints.value -= 1
  
  const action = gameState.currentLibraryAction
  let resultContent = action.content
  let gains = []
  
  // 根据选择执行不同的逻辑
  if (action.type === '了解武德') {
    // 根据当前的武术流派选择不同的内容
    // 这里简化处理，直接给全属性+1
    gains = [
      { type: 'strength', value: 1 },
      { type: 'martialArts', value: 1 },
      { type: 'personality', value: 1 },
      { type: 'mood', value: 1 }
    ]
  } else if (action.type === '参悟武技') {
    // 处理武技参悟的逻辑
    if (action.subChoice && action.subChoice.skill) {
      // 应用属性增益
      if (action.subChoice.gains) {
        gains = action.subChoice.gains
      }
      // 更新技能熟练度
      if (action.subChoice.skill && action.subChoice.proficiency) {
        updateSkillProficiency(action.subChoice.skill, action.subChoice.proficiency)
        resultContent += `\n${action.subChoice.skill}熟练度提升至${gameState.skillProficiency[action.subChoice.skill]}%`
      }
    }
  } else if (action.type === '了解武魂') {
    // 了解武魂的逻辑
    gains = [
      { type: 'personality', value: 2 },
      { type: 'mood', value: 3 }
    ]
  }
  
  // 应用增益
  if (gains.length > 0) {
    applyAttributeChanges(gains)
  }
  
  // 设置结果
  gameState.libraryResult = {
    content: resultContent,
    gains: gains
  }
}

// 获取可用技能列表
const getAvailableSkills = (style, skillType) => {
  if (!style || !skillType) return []
  
  const targetStyle = libraryContent.styles.find(s => s.name === style)
  if (!targetStyle) return []
  
  switch(skillType) {
    case '拳法':
      return targetStyle.fistSkills || []
    case '腿法':
      return targetStyle.legSkills || []
    case '武器':
      return targetStyle.weaponSkills || []
    default:
      return []
  }
}

// 取消藏书阁动作
const cancelLibraryAction = () => {
  gameState.currentLibraryAction = null
  gameState.libraryResult = null
}

// 选择选项
const selectChoice = (choice) => {
  gameState.selectedChoice = choice
  gameState.showChoiceResult = true
  
  // 处理条件内容和增益
  if (choice.conditionalContent) {
    // 查找满足条件的第一个内容
    const conditionalOption = choice.conditionalContent.find(option => {
      return typeof option.condition === 'function' && option.condition()
    })
    
    if (conditionalOption) {
      // 如果有条件内容，使用它的内容和增益
      choice.content = conditionalOption.content
      applyAttributeChanges(conditionalOption.gains)
    } else if (choice.gains) {
      // 否则使用默认增益
      applyAttributeChanges(choice.gains)
    }
  } else if (choice.gains) {
    // 普通选择的增益
    applyAttributeChanges(choice.gains)
  }
  
  // 应用关系变化
  if (choice.relationshipChanges) {
    applyRelationshipChanges(choice.relationshipChanges)
  }
  
  // 解锁新功能
  if (choice.unlockedFeatures) {
    choice.unlockedFeatures.forEach(feature => {
      if (!gameState.unlockedFeatures.includes(feature)) {
        gameState.unlockedFeatures.push(feature)
      }
    })
  }
  
  // 执行选择后的回调
  if (choice.onSelect) {
    choice.onSelect(choice)
  }
}

// 进入下一年
const goToNextYear = () => {
  // 检查行动点是否达到或超过5点，如果是则提示用户使用行动点
  if (actionPoints.value >= 5) {
    alert('行动点已达到上限5点，无法进入下一年。请先使用部分行动点！');
    return;
  }
  
  const currentStory = getCurrentStory()
  
  // 如果当前年份有选择但还没做出选择，则不进入下一年
  if (currentStory && currentStory.choices && !gameState.showChoiceResult) {
    return
  }
  
  // 如果当前年份有普通属性变化，应用这些变化
  if (currentStory && currentStory.gains) {
    applyAttributeChanges(currentStory.gains)
  }
  
  // 解锁新功能
  if (currentStory && currentStory.unlockedFeatures) {
    currentStory.unlockedFeatures.forEach(feature => {
      if (!gameState.unlockedFeatures.includes(feature)) {
        gameState.unlockedFeatures.push(feature)
      }
    })
  }
  
  // 重置选择状态
  gameState.selectedChoice = null
  gameState.showChoiceResult = false
  
  // 获取当前年份所有符合条件的故事
  const eligibleStories = character.lifeStory.filter(story => {
    return story.age === gameState.currentAge && 
           (!story.condition || (typeof story.condition === 'function' && story.condition()))
  });
  
  // 检查当前年份是否还有未显示的故事
  if (gameState.currentStoryIndex + 1 < eligibleStories.length) {
    // 显示下一个故事，不进入下一年
    gameState.currentStoryIndex += 1;
    return;
  }
  
  // 进入下一年
  // 特殊处理年龄跳跃逻辑
  let nextYear = gameState.currentAge + 1
  let ageChanged = false

  // Special jump logic for non-consecutive ages
  // First, check all non-consecutive age jumps regardless of branch
  if (gameState.currentAge === 9) {
    nextYear = 13
    gameState.currentAge = nextYear
    gameState.currentStoryIndex = 0
    ageChanged = true
  } else if (gameState.currentAge === 13) {
    nextYear = 15
    gameState.currentAge = nextYear
    gameState.currentStoryIndex = 0
    ageChanged = true
  } else if (gameState.currentAge === 17) {
    nextYear = 19
    gameState.currentAge = nextYear
    gameState.currentStoryIndex = 0
    ageChanged = true
  } else if (gameState.currentAge === 19) {
    nextYear = 22
    gameState.currentAge = nextYear
    gameState.currentStoryIndex = 0
    ageChanged = true
  } else if (gameState.permissions.hasIndependentOperation) {
    if (gameState.currentAge === 23) {
      nextYear = 24
      gameState.currentAge = nextYear
      gameState.currentStoryIndex = 0
      ageChanged = true
    } else if (gameState.currentAge === 24) {
      nextYear = 26
      gameState.currentAge = nextYear
      gameState.currentStoryIndex = 0
      ageChanged = true
    } else if (gameState.currentAge === 26) {
      nextYear = 28
      gameState.currentAge = nextYear
      gameState.currentStoryIndex = 0
      ageChanged = true
    } else if (gameState.currentAge === 28) {
      nextYear = 33
      gameState.currentAge = nextYear
      gameState.currentStoryIndex = 0
      ageChanged = true
    } else if (gameState.currentAge === 33) {
      nextYear = 35
      gameState.currentAge = nextYear
      gameState.currentStoryIndex = 0
      ageChanged = true
    } else if (gameState.currentAge === 35) {
      nextYear = 40
      gameState.currentAge = nextYear
      ageChanged = true
    } else if (gameState.currentAge === 40) {
      nextYear = 45
      gameState.currentAge = nextYear
      ageChanged = true
    } else if (gameState.currentAge === 45) {
      nextYear = 60
      gameState.currentAge = nextYear
      ageChanged = true
    } else if (gameState.currentAge === 60) {
      nextYear = 75
      gameState.currentAge = nextYear
      ageChanged = true
    }
  } else {
    // 留在馆内分支的年龄跳跃
    if (gameState.currentAge === 23) {
      nextYear = 27
      gameState.currentAge = nextYear
      gameState.currentStoryIndex = 0
      gameState.readyForNextYear = false
      ageChanged = true
    } else if (gameState.currentAge === 27) {
      nextYear = 30
      gameState.currentAge = nextYear
      ageChanged = true
    } else if (gameState.currentAge === 30) {
      nextYear = 33
      gameState.currentAge = nextYear
      ageChanged = true
    } else if (gameState.currentAge === 33) {
      nextYear = 35
      gameState.currentAge = nextYear
      ageChanged = true
    } else if (gameState.currentAge === 35) {
      nextYear = 40
      gameState.currentAge = nextYear
      ageChanged = true
    } else if (gameState.currentAge === 40) {
      nextYear = 50
      gameState.currentAge = nextYear
      ageChanged = true
    } else if (gameState.currentAge === 50) {
      nextYear = 70
      gameState.currentAge = nextYear
      ageChanged = true
    }
  }
  
  // 如果没有触发特殊跳跃，执行正常检查
  if (!ageChanged) {
    // 其他年龄正常检查
    const hasNextStory = character.lifeStory.some(story => {
      return story.age === nextYear && 
             (!story.condition || (typeof story.condition === 'function' && story.condition()))
    })
    
    if (hasNextStory) {
      gameState.currentAge = nextYear
      gameState.currentStoryIndex = 0
      ageChanged = true
    }
  }
  
  // 根据年龄设置行动点
  if (ageChanged) {
      // 根据不同年龄段设置行动点数量
      let gainPoints = 0
      
      // 根据用户提供的规则设置行动点
      switch(nextYear) {
        case 6: gainPoints = 1; break;  // 首次获得行动点
        case 7: gainPoints = 2; break;
        case 8: gainPoints = 1; break;
        case 9: gainPoints = 1; break;
        case 13: gainPoints = 2; break; // 原文写"行为点"，统一为行动点
        case 15: gainPoints = 1; break;
        case 16: gainPoints = 2; break;
        case 17: gainPoints = 1; break;
        case 19: gainPoints = 2; break;
        case 22: gainPoints = 3; break;
        case 23: gainPoints = 3; break; // 主线（分支节点前）
        case 27: 
          // 留在馆内分支
          if (!gameState.permissions.hasIndependentOperation) {
            gainPoints = 3;
          }
          break;
        case 30:
          // 留在馆内分支
          if (!gameState.permissions.hasIndependentOperation) {
            gainPoints = 2;
          }
          break;
        case 33: gainPoints = 2; break; // 主线（双分支共用）
        case 35: gainPoints = 2; break; // 双分支共用
        case 40:
          // 根据分支不同设置不同的行动点
          if (gameState.permissions.hasIndependentOperation) {
            gainPoints = 3; // 自立门户分支，原文写"三点行动点"
          } else {
            gainPoints = 2; // 留在馆内分支
          }
          break;
        case 50:
          // 留在馆内分支
          if (!gameState.permissions.hasIndependentOperation) {
            gainPoints = 2;
          }
          break;
        case 60:
          // 自立门户分支
          if (gameState.permissions.hasIndependentOperation) {
            gainPoints = 1;
          }
          break;
        case 75:
          // 自立门户分支
          if (gameState.permissions.hasIndependentOperation) {
            gainPoints = 1;
          }
          break;
        // 不在上面的年龄一律不加行动点
      }
      
      // 添加行动点，确保不超过最大限制
      if (gainPoints > 0) {
        actionPoints.value += gainPoints
        limitActionPoints()
      }
  }
}

// 继续到下一步
const continueToNextStep = () => {
  // 直接调用goToNextYear，内部已经会重置选择状态
  goToNextYear()
}

// 模拟游戏完成
const completeGame = () => {
  const currentStory = getCurrentStory()
  if (!currentStory || !currentStory.isGame) return;
  
  // 特殊处理马步平衡游戏
  if (currentStory.gameName === '基础考察——马步平衡') {
    // 创建马步平衡游戏界面
    const gameHtml = `
      <div id="horseStanceGame" style="
        width: 100%;
        height: 300px;
        background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
        position: relative;
        margin: 20px 0;
        border-radius: 10px;
        overflow: hidden;
        border: 2px solid #8B4513;
      ">
        <!-- 地面 -->
        <div style="
          position: absolute;
          bottom: 0;
          left: 0;
          width: 100%;
          height: 50px;
          background: #8B4513;
          display: flex;
          justify-content: center;
          align-items: flex-end;
        ">
          <!-- 梅花桩 -->
          <div style="
            width: 30px;
            height: 30px;
            background: #654321;
            border-radius: 50%;
            position: relative;
            box-shadow: 0 0 10px rgba(0,0,0,0.3);
          ">
            <!-- 玩家 -->
            <div id="player" style="
              width: 50px;
              height: 80px;
              position: absolute;
              top: -80px;
              left: -10px;
              transform-origin: center bottom;
            ">
              <!-- 玩家身体 -->
              <div style="
                width: 20px;
                height: 40px;
                background: #E63946;
                position: absolute;
                bottom: 0;
                left: 15px;
              "></div>
              <!-- 玩家头部 -->
              <div style="
                width: 20px;
                height: 20px;
                background: #FFD700;
                border-radius: 50%;
                position: absolute;
                top: 0;
                left: 15px;
              "></div>
              <!-- 玩家手臂 -->
              <div style="
                width: 10px;
                height: 30px;
                background: #E63946;
                position: absolute;
                top: 10px;
                left: 5px;
                transform: rotate(45deg);
              "></div>
              <div style="
                width: 10px;
                height: 30px;
                background: #E63946;
                position: absolute;
                top: 10px;
                left: 35px;
                transform: rotate(-45deg);
              "></div>
            </div>
          </div>
        </div>
        
        <!-- 状态显示 -->
        <div style="
          position: absolute;
          top: 10px;
          left: 10px;
          background: rgba(0,0,0,0.7);
          color: white;
          padding: 10px;
          border-radius: 5px;
          font-family: Arial;
        ">
          <div>时间: <span id="timer" style="font-size: 14px;">60</span>s</div>
          <div>平衡度: <span id="balance" style="font-size: 14px;">50%</span></div>
        </div>
        
        <!-- 平衡滑块 -->
        <div id="sliderContainer" style="
          position: absolute;
          bottom: 30px; /* 移动到页面最下方 */
          width: 80%;
          height: 20px;
          background: rgba(0,0,0,0.3);
          border-radius: 15px;
          overflow: hidden;
          display: flex;
          box-shadow: 0 4px 8px rgba(0,0,0,0.5);
        ">
          <!-- 左偏马区域 (40%) -->
          <div style="
            width: 40%;
            height: 100%;
            background: linear-gradient(90deg, #FF9800 0%, #FFB74D 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            color: #FFF;
            font-weight: bold;
            font-size: 14px;
          ">左偏马</div>
          
          <!-- 中正马区域 (20%) -->
          <div style="
            width: 20%;
            height: 100%;
            background: linear-gradient(90deg, #4CAF50 0%, #66BB6A 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            color: #FFF;
            font-weight: bold;
            font-size: 14px;
          ">中正马</div>
          
          <!-- 右偏马区域 (40%) -->
          <div style="
            width: 40%;
            height: 100%;
            background: linear-gradient(90deg, #2196F3 0%, #64B5F6 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            color: #FFF;
            font-weight: bold;
            font-size: 14px;
          ">右偏马</div>
        </div>
        
        <!-- 滑块按钮 -->
        <div id="sliderButton" style="
          position: absolute;
          bottom: 27px; /* 调整到滑块容器中间 */
          width: 24px;
          height: 24px;
          background: linear-gradient(135deg, #E63946 0%, #C1121F 100%);
          border: 2px solid #FFF;
          border-radius: 50%;
          cursor: pointer;
          box-shadow: 0 6px 12px rgba(0,0,0,0.5);
          transition: left 0.2s ease;
        ></div>
        
        <!-- 控制提示 -->
        <div style="
          position: absolute;
          top: 10px;
          right: 10px;
          background: rgba(0,0,0,0.7);
          color: white;
          padding: 10px;
          border-radius: 5px;
          font-family: Arial;
        ">
          <div>←: 向左移动</div>
          <div>→: 向右移动</div>
          <div>控制滑块在中间区域保持平衡</div>
        </div>
        
        <!-- 平衡提示 -->
        <div id="balanceStatus" style="
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
          font-size: 24px;
          font-weight: bold;
          color: #E63946;
          text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
          opacity: 0;
          transition: opacity 0.3s ease;
          z-index: 20;
        ">保持平衡！</div>
        
        <!-- 触摸控制按钮 - 三点布局 -->
        
        <!-- 左侧 - 左偏马 -->
        <button id="btnLeft" style="
          position: absolute;
          bottom: 80px;
          left: 60px;
          width: 80px;
          height: 80px;
          border-radius: 50%;
          background: linear-gradient(135deg, #2196F3 0%, #1976D2 100%);
          color: white;
          font-size: 16px;
          font-weight: bold;
          border: 3px solid #E3F2FD;
          cursor: pointer;
          box-shadow: 0 6px 12px rgba(0,0,0,0.4), 0 0 0 3px rgba(255,255,255,0.15) inset;
          user-select: none;
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          gap: 4px;
          touch-action: manipulation;
          z-index: 10;
          transition: all 0.3s ease;
        ">
          ←
          <span style="font-size: 12px;">左偏马</span>
        </button>
        

        
        <!-- 右侧 - 右偏马 -->
        <button id="btnRight" style="
          position: absolute;
          bottom: 80px;
          right: 60px;
          width: 80px;
          height: 80px;
          border-radius: 50%;
          background: linear-gradient(135deg, #FF9800 0%, #F57C00 100%);
          color: white;
          font-size: 16px;
          font-weight: bold;
          border: 3px solid #FFF8E1;
          cursor: pointer;
          box-shadow: 0 6px 12px rgba(0,0,0,0.4), 0 0 0 3px rgba(255,255,255,0.15) inset;
          user-select: none;
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          gap: 4px;
          touch-action: manipulation;
          z-index: 10;
          transition: all 0.3s ease;
        ">
          →
          <span style="font-size: 12px;">右偏马</span>
        </button>
      </div>
      
      <div id="gameControls" style="
        text-align: center;
        margin-top: 20px;
      ">
        <button id="startGame" class="action-btn" style="margin-right: 10px;">开始训练</button>
      </div>`;
      
      // 显示游戏界面
      const gameSection = document.querySelector('.game-section');
      if (gameSection) {
        // 移除可能存在的旧游戏内容
        const existingGameContent = gameSection.querySelector('#horseStanceGame, #gameControls');
        if (existingGameContent) {
          existingGameContent.remove();
        }
        
        // 插入游戏HTML
        const tempContainer = document.createElement('div');
        tempContainer.innerHTML = gameHtml;
        
        // 找到"完成练习"按钮的位置，在其前面插入游戏内容
        const completeButton = gameSection.querySelector('.action-btn');
        gameSection.insertBefore(tempContainer.querySelector('#horseStanceGame'), completeButton);
        gameSection.insertBefore(tempContainer.querySelector('#gameControls'), completeButton);
        
        // 隐藏原有的完成按钮
        if (completeButton) {
          completeButton.style.display = 'none';
        }
      }
      
      // 游戏逻辑变量
      let isPlaying = false;
      let timeLeft = 60;
      let balance = 50; // 初始平衡度设为50%
      let sliderPosition = 50; // 滑块位置，范围0-100
      let isMovingLeft = false;
      let isMovingRight = false;
      let moveInterval;
      let timerInterval;
      const keyMap = {
        'ArrowLeft': 'left',
        'ArrowRight': 'right'
      };
      
      // 获取DOM元素
      const timerEl = document.getElementById('timer');
      const balanceEl = document.getElementById('balance');
      const playerEl = document.getElementById('player');
      const startGameBtn = document.getElementById('startGame');
      const btnLeft = document.getElementById('btnLeft');
      const btnRight = document.getElementById('btnRight');
      const sliderContainer = document.getElementById('sliderContainer');
      const sliderButton = document.getElementById('sliderButton');
      
      // 开始游戏
      if (startGameBtn) {
        startGameBtn.addEventListener('click', function() {
          isPlaying = true;
          startGameBtn.disabled = true;
          
          // 初始化滑块位置
          sliderPosition = 50;
          isMovingLeft = false;
          isMovingRight = false;
          
          // 初始化滑块按钮位置
          if (sliderButton && sliderContainer) {
            const containerWidth = sliderContainer.offsetWidth;
            sliderButton.style.left = `${(sliderPosition / 100) * containerWidth}px`;
          }
          
          // 开始倒计时
          timerInterval = setInterval(function() {
            timeLeft--;
            if (timerEl) {
              timerEl.textContent = timeLeft;
            }
            
            // 游戏结束条件
            if (timeLeft <= 0 || balance <= 0) {
              // 时间耗尽时：如果平衡度>=50则成功，否则失败
              // 平衡度耗尽时：直接失败
              const success = timeLeft <= 0 && balance >= 50;
              endGame(success);
            }
          }, 1000);
          
          // 开始自动移动滑块
          // 增加间隔时间，使每次移动有更明显的视觉效果
          moveInterval = setInterval(moveSlider, 80);
        });
      }
      
      // 结束游戏功能已整合到自动倒计时结束中
      
      // 处理键盘输入
      document.addEventListener('keydown', handleKeydown);
      
      // 处理按钮点击 - 完全重写，使用最简单直接的方式
      // 先移除可能存在的旧事件监听器
      function resetButtonEvents(button) {
        if (!button) return;
        // 使用克隆元素方法移除所有事件监听器
        const clone = button.cloneNode(true);
        button.parentNode.replaceChild(clone, button);
        return clone;
      }
      
      // 重置按钮并获取新引用
      const btnLeftNew = resetButtonEvents(btnLeft);
      const btnRightNew = resetButtonEvents(btnRight);
      
      // 重新获取引用
      btnLeft = btnLeftNew || document.getElementById('btnLeft');
      btnRight = btnRightNew || document.getElementById('btnRight');
      
      // 为左右按钮添加完全新的事件处理
      function handleButtonPress(button, direction) {
        if (!isPlaying) {
          console.log('按钮点击但游戏未开始');
          return;
        }
        
        // 直接、大幅移动滑块
        const oldPosition = sliderPosition;
        if (direction === 'left') {
          sliderPosition = Math.max(0, sliderPosition - 30); // 极大增加步长到30
        } else {
          sliderPosition = Math.min(100, sliderPosition + 30); // 极大增加步长到30
        }
        
        // 添加调试日志
        console.log('按钮点击:', direction, '旧位置:', oldPosition, '新位置:', sliderPosition);
        
        // 立即更新滑块位置和视觉效果
        if (sliderButton && sliderContainer) {
          const containerWidth = sliderContainer.offsetWidth;
          const buttonWidth = sliderButton.offsetWidth;
          const buttonPosition = (sliderPosition / 100) * containerWidth;
          const leftPosition = buttonPosition - buttonWidth / 2;
          sliderButton.style.left = `${leftPosition}px`;
          console.log('更新滑块视觉位置:', leftPosition, 'px');
        }
        
        // 更新平衡度和玩家显示
        updateBalanceAndPlayer();
        
        // 应用视觉反馈
        button.style.transform = 'scale(0.8)';
        button.style.boxShadow = '0 2px 4px rgba(0,0,0,0.3)';
        button.style.backgroundColor = '#3a7bc8';
      }
      
      function handleButtonRelease(button) {
        // 恢复按钮状态
        button.style.transform = 'scale(1)';
        button.style.boxShadow = '0 6px 12px rgba(0,0,0,0.4), 0 0 0 3px rgba(255,255,255,0.15) inset';
        button.style.backgroundColor = '#4a90e2';
      }
      
      // 添加新的事件监听器
      if (btnLeft) {
        btnLeft.addEventListener('mousedown', function() { handleButtonPress(this, 'left'); });
        btnLeft.addEventListener('touchstart', function(e) { e.preventDefault(); handleButtonPress(this, 'left'); });
        btnLeft.addEventListener('mouseup', function() { handleButtonRelease(this); });
        btnLeft.addEventListener('touchend', function() { handleButtonRelease(this); });
        btnLeft.addEventListener('touchcancel', function() { handleButtonRelease(this); });
      }
      
      if (btnRight) {
        btnRight.addEventListener('mousedown', function() { handleButtonPress(this, 'right'); });
        btnRight.addEventListener('touchstart', function(e) { e.preventDefault(); handleButtonPress(this, 'right'); });
        btnRight.addEventListener('mouseup', function() { handleButtonRelease(this); });
        btnRight.addEventListener('touchend', function() { handleButtonRelease(this); });
        btnRight.addEventListener('touchcancel', function() { handleButtonRelease(this); });
      }
      
      // 处理键盘输入
      document.addEventListener('keydown', function(e) {
        if (!isPlaying) return;
        
        // 阻止默认行为，避免页面滚动
        if (e.key === 'ArrowLeft' || e.key === 'ArrowRight') {
          e.preventDefault();
        }
        
        if (e.key === 'ArrowLeft') {
          isMovingLeft = true;
        } else if (e.key === 'ArrowRight') {
          isMovingRight = true;
        }
      });
      
      document.addEventListener('keyup', function(e) {
        if (e.key === 'ArrowLeft') {
          isMovingLeft = false;
        } else if (e.key === 'ArrowRight') {
          isMovingRight = false;
        }
      });
      
      // 自动移动滑块 - 使用直接控制方式
      function moveSlider() {
        if (!isPlaying) return;
        
        const oldPosition = sliderPosition;
        
        // 玩家控制优先级最高
        if (isMovingLeft) {
          // 直接大幅向左移动，无需任何平滑处理，确保控制效果明显
          sliderPosition = Math.max(0, sliderPosition - 15);
          console.log('键盘长按左移:', oldPosition, '→', sliderPosition);
        } else if (isMovingRight) {
          // 直接大幅向右移动，无需任何平滑处理，确保控制效果明显
          sliderPosition = Math.min(100, sliderPosition + 15);
          console.log('键盘长按右移:', oldPosition, '→', sliderPosition);
        } else {
          // 只有在玩家没有操作时才进行随机移动
          // 显著降低随机强度，确保玩家控制始终占主导
          const edgeEffect = 1 + (Math.abs(sliderPosition - 50) / 50) * 0.5; // 大幅降低边缘效应
          const randomIntensity = 0.8; // 大幅降低随机强度
          const randomOffset = (Math.random() - 0.5) * randomIntensity * edgeEffect;
          sliderPosition += randomOffset;
          console.log('随机移动:', oldPosition, '→', sliderPosition, '(偏移:', randomOffset, ')');
        }
        
        // 确保滑块位置在有效范围内
        sliderPosition = Math.max(0, Math.min(100, sliderPosition));
        
        // 更新平衡度、滑块和玩家显示
        updateBalanceAndPlayer();
        
        // 检查游戏状态
        if (balance <= 0) {
          endGame(false);
        }
      }
      
      // 更新平衡度、滑块和玩家显示
      function updateBalanceAndPlayer() {
        // 根据滑块位置更新平衡度
        // 当滑块在40%-60%范围内时增加平衡度，否则减少平衡度
        if (sliderPosition >= 40 && sliderPosition <= 60) {
          // 在成功区域内
          balance = Math.min(100, balance + 0.5);
          
          // 如果在大成功区域(45%-55%)，增加更多平衡度
          if (sliderPosition >= 45 && sliderPosition <= 55) {
            balance = Math.min(100, balance + 0.5);
          }
        } else {
          // 在失败区域
          balance = Math.max(0, balance - 1);
        }
        
        // 更新平衡度显示
        if (balanceEl) {
          balanceEl.textContent = Math.round(balance) + '%';
        }
        
        // 更新滑块按钮位置
        if (sliderButton && sliderContainer) {
          // 计算滑块容器的宽度
          const containerWidth = sliderContainer.offsetWidth;
          // 计算按钮位置，确保按钮在容器内居中
          const buttonPosition = (sliderPosition / 100) * containerWidth;
          // 更新按钮位置，减去按钮宽度的一半以居中
          const buttonWidth = sliderButton.offsetWidth;
          sliderButton.style.left = `${buttonPosition - buttonWidth / 2}px`;
        }
        
        // 根据滑块位置更新玩家显示
        if (playerEl) {
          // 将滑块位置映射到旋转角度：0 → -15度，100 → 15度（增加倾斜角度）
          const rotateAngle = (sliderPosition - 50) * 0.3;
          playerEl.style.transform = `rotate(${rotateAngle}deg)`;
          
          // 当平衡度低时添加抖动动画
          if (balance < 30) {
            playerEl.style.animation = 'shake 0.5s ease-in-out';
            setTimeout(function() {
              if (playerEl && isPlaying) {
                playerEl.style.animation = '';
              }
            }, 500);
          }
        }
      }
      
      // 添加抖动动画样式
      const styleElement = document.createElement('style');
      styleElement.textContent = `
        @keyframes shake {
          0%, 100% { transform: rotate(0deg); }
          25% { transform: rotate(-5deg); }
          75% { transform: rotate(5deg); }
        }
      `;
      document.head.appendChild(styleElement);
      
      // 结束游戏并返回结果
      function endGame(success) {
        clearInterval(timerInterval);
        clearInterval(moveInterval);
        isPlaying = false;
        
        // 重置移动状态
        isMovingLeft = false;
        isMovingRight = false;
        
        if (success) {
          // 根据滑块位置决定奖励
          let message = '恭喜！你成功完成了马步平衡训练！';
          
          // 应用奖励
          if (currentStory && currentStory.afterGameGains) {
            let actualGains = null;
            let specialFlag = null;
            
            // 大成功：滑块位置在45%-55%范围内
            if (sliderPosition >= 45 && sliderPosition <= 55) {
              // 大成功 - 双倍奖励
              message += '\n大成功！中正马姿势完美！获得双倍奖励：武德+2、体魄+2';
              actualGains = currentStory.afterGameGains.map(gain => ({
                ...gain,
                value: gain.value * 2
              }));
              specialFlag = 'perfect';
            } 
            // 普通成功：滑块位置在40%-60%范围内
            else if (sliderPosition >= 40 && sliderPosition <= 60) {
              // 普通成功 - 标准奖励
              message += '\n姿势稳定！获得奖励：武德+1、体魄+1';
              actualGains = currentStory.afterGameGains;
            } 
            // 非成功区域（这个情况理论上不应该出现，因为success参数由平衡度决定）
            else {
              // 无奖励
              message += '\n姿势不够稳定，未获得奖励。';
              actualGains = [];
            }
            
            // 应用增益
            if (actualGains.length > 0) {
              applyAttributeChanges(actualGains);
              
              // 记录事件
              gameState.history.push({
                year: gameState.currentAge,
                event: currentStory.gameName,
                gains: actualGains,
                ...(specialFlag ? { special: specialFlag } : {})
              });
            }
          }
          
          alert(message);
          // 进入下一年
          goToNextYear();
        } else {
          alert('训练失败！平衡度耗尽，你从梅花桩上摔了下来。请重新尝试。');
          // 重置游戏状态
          timeLeft = 60;
          balance = 50;
          sliderPosition = 50;
          isMovingLeft = false;
          isMovingRight = false;
          if (timerEl) timerEl.textContent = timeLeft;
          if (balanceEl) balanceEl.textContent = balance + '%';
          if (playerEl) playerEl.style.transform = 'rotate(0deg)';
          if (startGameBtn) startGameBtn.disabled = false;
        }
      }
      
      
  }
  
  // 特殊处理9岁棍法·缠丝游戏
  if (currentStory.gameName === '特色技动作游戏——棍法·缠丝') {
    // 创建棍法·缠丝游戏界面
    const gameHtml = `
      <div id="willowGame" style="
        width: 100%;
        height: 400px;
        background: linear-gradient(to bottom, #87CEEB, #E0F7FA);
        position: relative;
        margin: 20px 0;
        border-radius: 10px;
        overflow: hidden;
        border: 2px solid #8B4513;
      ">
        <!-- 分数面板 -->
        <div style="
          position: absolute;
          top: 10px;
          left: 10px;
          background: rgba(0,0,0,0.7);
          color: white;
          padding: 10px;
          border-radius: 5px;
          font-family: Arial;
        ">
          <div>当前分数: <span id="currentScore">0</span></div>
          <div>历史最高分: <span id="highScore">0</span></div>
          <div>速度倍率: <span id="speedMultiplier">×1.0</span></div>
        </div>
        
        <!-- 熊角色 -->
        <div id="bearPlayer" style="
          position: absolute;
          bottom: 30px;
          left: 50%;
          width: 80px;
          height: 100px;
          transform: translateX(-50%);
          transition: left 0.05s linear;
          z-index: 10;
        ">
          <!-- 熊头部 -->
          <div style="
            width: 60px;
            height: 60px;
            background: #8B4513;
            border-radius: 50%;
            position: absolute;
            top: 0;
            left: 10px;
          ">
            <div style="width: 10px; height: 10px; background: white; border-radius: 50%; position: absolute; top: 15px; left: 10px;"></div>
            <div style="width: 10px; height: 10px; background: white; border-radius: 50%; position: absolute; top: 15px; left: 40px;"></div>
            <div style="width: 20px; height: 5px; background: black; border-radius: 5px; position: absolute; top: 35px; left: 20px;"></div>
          </div>
          <!-- 熊身体 -->
          <div style="
            width: 80px;
            height: 60px;
            background: #8B4513;
            border-radius: 40px;
            position: absolute;
            top: 40px;
            left: 0;
          "></div>
        </div>
        
        <!-- 地面 -->
        <div style="
          position: absolute;
          bottom: 0;
          left: 0;
          width: 100%;
          height: 20px;
          background: #654321;
        "></div>
        
        <!-- 操作指引 -->
        <div style="
          position: absolute;
          bottom: 130px;
          left: 50%;
          transform: translateX(-50%);
          background: rgba(0,0,0,0.7);
          color: white;
          padding: 8px 15px;
          border-radius: 20px;
          font-family: Arial;
          font-size: 14px;
          white-space: nowrap;
        ">
          滑动屏幕控制熊
        </div>
        
        <!-- 触摸控制区域 -->
        <div id="touchAreaLeft" style="
          position: absolute;
          top: 0;
          left: 0;
          width: 50%;
          height: 100%;
          z-index: 5;
          opacity: 0;
        "></div>
        <div id="touchAreaRight" style="
          position: absolute;
          top: 0;
          right: 0;
          width: 50%;
          height: 100%;
          z-index: 5;
          opacity: 0;
        "></div>
        
        <!-- 游戏结束面板 -->
        <div id="gameOverPanel" style="
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
          background: rgba(0,0,0,0.8);
          color: white;
          padding: 20px;
          border-radius: 10px;
          text-align: center;
          display: none;
          z-index: 20;
        ">
          <h3 style="margin-top: 0;">游戏结束</h3>
          <p>最终得分: <span id="finalScore">0</span></p>
          <button id="restartGame" style="
            margin-top: 10px;
            padding: 8px 16px;
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
          ">再来一局</button>
          <button id="finishGame" style="
            margin-top: 10px;
            margin-left: 10px;
            padding: 8px 16px;
            background: #2196F3;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
          ">完成训练</button>
        </div>
      </div>
      
      <div id="willowGameControls" style="
        text-align: center;
        margin-top: 20px;
      ">
        <button id="startWillowGame" class="action-btn" style="margin-right: 10px;">开始训练</button>
      </div>`;
    
    // 显示游戏界面
    const gameSection = document.querySelector('.game-section');
    if (gameSection) {
      // 移除可能存在的旧游戏内容
      const existingGameContent = gameSection.querySelector('#willowGame, #willowGameControls');
      if (existingGameContent) {
        existingGameContent.remove();
      }
      
      // 插入游戏HTML
      const tempContainer = document.createElement('div');
      tempContainer.innerHTML = gameHtml;
      
      // 找到"完成练习"按钮的位置，在其前面插入游戏内容
      const completeButton = gameSection.querySelector('.action-btn');
      gameSection.insertBefore(tempContainer.querySelector('#willowGame'), completeButton);
      gameSection.insertBefore(tempContainer.querySelector('#willowGameControls'), completeButton);
      
      // 隐藏原有的完成按钮
      if (completeButton) {
        completeButton.style.display = 'none';
      }
    }
    
    // 游戏逻辑变量
    let isPlaying = false;
    let score = 0;
    let highScore = localStorage.getItem('willowGameHighScore') || 0;
    let leaves = [];
    let leafSpeed = 200; // 初始速度，像素/秒
    let speedMultiplier = 1.0;
    let gameTimer = null;
    let leafTimer = null;
    let speedTimer = null;
    let missedLeaves = 0;
    let bearX = 50; // 百分比位置
    const bearSpeed = 300; // 像素/秒
    const gameContainerEl = document.getElementById('willowGame');
    
    // 获取DOM元素
    const bearEl = document.getElementById('bearPlayer');
    const currentScoreEl = document.getElementById('currentScore');
    const highScoreEl = document.getElementById('highScore');
    const speedMultiplierEl = document.getElementById('speedMultiplier');
    const startGameBtn = document.getElementById('startWillowGame');
    const gameOverPanel = document.getElementById('gameOverPanel');
    const finalScoreEl = document.getElementById('finalScore');
    const restartGameBtn = document.getElementById('restartGame');
    const finishGameBtn = document.getElementById('finishGame');
    const touchAreaLeft = document.getElementById('touchAreaLeft');
    const touchAreaRight = document.getElementById('touchAreaRight');
    
    // 更新最高分显示
    if (highScoreEl) {
      highScoreEl.textContent = highScore;
    }
    
    // 开始游戏
    if (startGameBtn) {
      startGameBtn.addEventListener('click', startGame);
    }
    
    // 重新开始游戏
    if (restartGameBtn) {
      restartGameBtn.addEventListener('click', restartGame);
    }
    
    // 完成游戏
    if (finishGameBtn) {
      finishGameBtn.addEventListener('click', finishGameSession);
    }
    
    // 处理键盘输入
    document.addEventListener('keydown', handleWillowKeydown);
    document.addEventListener('keyup', handleWillowKeyup);
    
    // 触摸区域控制
    let touchMovingLeft = false;
    let touchMovingRight = false;
    
    if (touchAreaLeft) {
      touchAreaLeft.addEventListener('touchstart', () => touchMovingLeft = true);
      touchAreaLeft.addEventListener('touchend', () => touchMovingLeft = false);
      touchAreaLeft.addEventListener('mousedown', () => touchMovingLeft = true);
      touchAreaLeft.addEventListener('mouseup', () => touchMovingLeft = false);
    }
    
    if (touchAreaRight) {
      touchAreaRight.addEventListener('touchstart', () => touchMovingRight = true);
      touchAreaRight.addEventListener('touchend', () => touchMovingRight = false);
      touchAreaRight.addEventListener('mousedown', () => touchMovingRight = true);
      touchAreaRight.addEventListener('mouseup', () => touchMovingRight = false);
    }
    
    // 移动方向状态
    let moveLeft = false;
    let moveRight = false;
    
    function startGame() {
      isPlaying = true;
      score = 0;
      missedLeaves = 0;
      leaves = [];
      leafSpeed = 200;
      speedMultiplier = 1.0;
      bearX = 50;
      
      // 更新UI
      if (currentScoreEl) currentScoreEl.textContent = score;
      if (speedMultiplierEl) speedMultiplierEl.textContent = `×${speedMultiplier.toFixed(1)}`;
      updateBearPosition();
      
      // 隐藏游戏结束面板
      if (gameOverPanel) gameOverPanel.style.display = 'none';
      
      // 禁用开始按钮
      if (startGameBtn) startGameBtn.disabled = true;
      
      // 开始游戏主循环
      const lastTime = { value: performance.now() };
      gameTimer = requestAnimationFrame(function gameLoop(timestamp) {
        const deltaTime = (timestamp - lastTime.value) / 1000; // 转换为秒
        lastTime.value = timestamp;
        
        if (!isPlaying) return;
        
        // 处理熊移动
        handleBearMovement(deltaTime);
        
        // 更新柳叶位置
        updateLeaves(deltaTime);
        
        // 继续游戏循环
        gameTimer = requestAnimationFrame(gameLoop);
      });
      
      // 定时生成柳叶
      leafTimer = setInterval(createLeaf, 1500);
      
      // 定时增加速度
      speedTimer = setInterval(increaseSpeed, 10000);
      
      // 立即生成第一个柳叶
      createLeaf();
    }
    
    function handleWillowKeydown(e) {
      if (!isPlaying) return;
      
      if (e.key === 'ArrowLeft' || e.key === 'a' || e.key === 'A') {
        moveLeft = true;
      } else if (e.key === 'ArrowRight' || e.key === 'd' || e.key === 'D') {
        moveRight = true;
      }
    }
    
    function handleWillowKeyup(e) {
      if (e.key === 'ArrowLeft' || e.key === 'a' || e.key === 'A') {
        moveLeft = false;
      } else if (e.key === 'ArrowRight' || e.key === 'd' || e.key === 'D') {
        moveRight = false;
      }
    }
    
    function handleBearMovement(deltaTime) {
      if (!isPlaying || !gameContainerEl) return;
      
      const containerWidth = gameContainerEl.offsetWidth;
      const moveDistance = bearSpeed * deltaTime;
      
      // 处理键盘和触摸控制
      if ((moveLeft || touchMovingLeft) && bearX > 5) {
        bearX -= (moveDistance / containerWidth) * 100;
      }
      if ((moveRight || touchMovingRight) && bearX < 95) {
        bearX += (moveDistance / containerWidth) * 100;
      }
      
      // 限制在边界内
      bearX = Math.max(5, Math.min(95, bearX));
      
      // 更新熊的位置
      updateBearPosition();
    }
    
    function updateBearPosition() {
      if (!bearEl || !gameContainerEl) return;
      
      const containerWidth = gameContainerEl.offsetWidth;
      const bearWidth = bearEl.offsetWidth;
      const leftPosition = (bearX / 100) * containerWidth - bearWidth / 2;
      
      bearEl.style.left = `${leftPosition}px`;
    }
    
    function createLeaf() {
      if (!isPlaying || !gameContainerEl) return;
      
      const containerWidth = gameContainerEl.offsetWidth;
      
      // 创建柳叶元素
      const leaf = document.createElement('div');
      leaf.className = 'willowLeaf';
      leaf.style.position = 'absolute';
      leaf.style.width = '20px';
      leaf.style.height = '30px';
      leaf.style.backgroundColor = '#8BC34A';
      leaf.style.borderRadius = '50% 50% 20% 20%';
      leaf.style.top = '-30px';
      leaf.style.left = `${Math.random() * (containerWidth - 20)}px`;
      leaf.style.zIndex = '5';
      
      // 随机旋转角度
      leaf.style.transform = `rotate(${Math.random() * 360}deg)`;
      
      // 添加到容器
      gameContainerEl.appendChild(leaf);
      
      // 记录柳叶信息
      leaves.push({
        element: leaf,
        x: parseFloat(leaf.style.left),
        y: -30,
        width: 20,
        height: 30,
        rotation: Math.random() * 360
      });
    }
    
    function updateLeaves(deltaTime) {
      if (!isPlaying || !gameContainerEl) return;
      
      const containerHeight = gameContainerEl.offsetHeight;
      const groundY = containerHeight - 20; // 地面位置
      
      // 更新每个柳叶的位置
      for (let i = leaves.length - 1; i >= 0; i--) {
        const leaf = leaves[i];
        
        // 更新位置
        leaf.y += leafSpeed * speedMultiplier * deltaTime;
        leaf.element.style.top = `${leaf.y}px`;
        
        // 更新旋转
        leaf.rotation += 45 * deltaTime;
        leaf.element.style.transform = `rotate(${leaf.rotation}deg)`;
        
        // 检查碰撞
        if (checkLeafCollision(leaf)) {
          // 接住柳叶
          collectLeaf(i);
        } 
        // 检查是否落地
        else if (leaf.y > groundY) {
          // 错过柳叶
          missLeaf(i);
        }
      }
    }
    
    function checkLeafCollision(leaf) {
      if (!bearEl || !gameContainerEl) return false;
      
      const bearRect = bearEl.getBoundingClientRect();
      const leafRect = leaf.element.getBoundingClientRect();
      
      // 简单的矩形碰撞检测
      return !(leafRect.right < bearRect.left || 
               leafRect.left > bearRect.right || 
               leafRect.bottom < bearRect.top || 
               leafRect.top > bearRect.bottom);
    }
    
    function collectLeaf(index) {
      const leaf = leaves[index];
      
      // 移除柳叶元素
      if (leaf.element.parentNode) {
        leaf.element.parentNode.removeChild(leaf.element);
      }
      
      // 从数组中移除
      leaves.splice(index, 1);
      
      // 增加分数
      score++;
      if (currentScoreEl) {
        currentScoreEl.textContent = score;
      }
      
      // 更新最高分
      if (score > highScore) {
        highScore = score;
        localStorage.setItem('willowGameHighScore', highScore);
        if (highScoreEl) {
          highScoreEl.textContent = highScore;
        }
      }
    }
    
    function missLeaf(index) {
      const leaf = leaves[index];
      
      // 移除柳叶元素
      if (leaf.element.parentNode) {
        leaf.element.parentNode.removeChild(leaf.element);
      }
      
      // 从数组中移除
      leaves.splice(index, 1);
      
      // 增加错过次数
      missedLeaves++;
      
      // 扣分
      score = Math.max(0, score - 1);
      if (currentScoreEl) {
        currentScoreEl.textContent = score;
      }
      
      // 检查游戏结束条件
      if (score <= 0 && missedLeaves >= 3) {
        endGame();
      }
    }
    
    function increaseSpeed() {
      if (!isPlaying) return;
      
      // 增加速度，最大600px/s
      leafSpeed = Math.min(600, leafSpeed + 50);
      speedMultiplier = leafSpeed / 200;
      
      if (speedMultiplierEl) {
        speedMultiplierEl.textContent = `×${speedMultiplier.toFixed(1)}`;
      }
    }
    
    function endGame() {
      isPlaying = false;
      
      // 清除所有定时器
      if (gameTimer) cancelAnimationFrame(gameTimer);
      if (leafTimer) clearInterval(leafTimer);
      if (speedTimer) clearInterval(speedTimer);
      
      // 显示游戏结束面板
      if (finalScoreEl) finalScoreEl.textContent = score;
      if (gameOverPanel) gameOverPanel.style.display = 'block';
      
      // 重置移动状态
      moveLeft = false;
      moveRight = false;
      touchMovingLeft = false;
      touchMovingRight = false;
    }
    
    function restartGame() {
      // 清除所有柳叶
      leaves.forEach(leaf => {
        if (leaf.element.parentNode) {
          leaf.element.parentNode.removeChild(leaf.element);
        }
      });
      leaves = [];
      
      // 重新开始游戏
      startGame();
    }
    
    function finishGameSession() {
      // 清除所有定时器
      if (gameTimer) cancelAnimationFrame(gameTimer);
      if (leafTimer) clearInterval(leafTimer);
      if (speedTimer) clearInterval(speedTimer);
      
      // 清除所有柳叶
      leaves.forEach(leaf => {
        if (leaf.element.parentNode) {
          leaf.element.parentNode.removeChild(leaf.element);
        }
      });
      leaves = [];
      
      // 重新启用原始的完成按钮
      const completeButton = gameSection.querySelector('.action-btn:not(#startWillowGame)');
      if (completeButton) {
        completeButton.style.display = 'inline-block';
      }
      
      // 移除游戏元素
      if (gameContainerEl.parentNode) {
        gameContainerEl.parentNode.removeChild(gameContainerEl);
      }
      
      const controlsContainer = document.getElementById('willowGameControls');
      if (controlsContainer && controlsContainer.parentNode) {
        controlsContainer.parentNode.removeChild(controlsContainer);
      }
      
      // 重置移动状态
      moveLeft = false;
      moveRight = false;
      touchMovingLeft = false;
      touchMovingRight = false;
      
      // 成功完成游戏，应用属性加成
      const afterGameGains = currentStory.afterGameGains || [];
      applyGains(afterGameGains);
      
      // 解锁特色技
      if (!gameState.unlockedFeatures.includes('棍法·缠丝')) {
        gameState.unlockedFeatures.push('棍法·缠丝');
        // 额外增加体魄属性
        strengthValue.value += 1;
      }
    }
    
    // 应用属性加成的辅助函数
    function applyGains(gains) {
      gains.forEach(gain => {
        switch (gain.type) {
          case 'strength':
            strengthValue.value += gain.value;
            break;
          case 'martialArts':
            martialArtsValue.value += gain.value;
            break;
          case 'personality':
            personalityValue.value += gain.value;
            break;
          case 'mood':
            moodValue.value += gain.value;
            break;
          case 'perseverance':
            perseveranceValue.value += gain.value;
            break;
          case 'reputation':
            reputationValue.value += gain.value;
            break;
        }
      });
    }
  }
  
  return; // 提前返回，不执行下面的通用逻辑
  }
  
  // 特殊处理7岁棍法小游戏
  const currentStory = getCurrentStory();
  if (currentStory && currentStory.gameName === '学习拳法') {
    // 创建棍法小游戏界面
    const gameHtml = `
      <div id="staffGame" style="
        width: 100%;
        height: 400px;
        background: linear-gradient(to bottom, #87CEEB, #E0F7FA);
        position: relative;
        margin: 20px 0;
        border-radius: 10px;
        overflow: hidden;
        border: 2px solid #8B4513;
      ">
        <!-- 远处小人区域 -->
        <div style="
          position: absolute;
          top: 50px;
          left: 0;
          width: 100%;
          height: 150px;
          display: flex;
          justify-content: space-around;
          align-items: center;
        ">
          <!-- 左侧小人 - 左冲拳 -->
          <div id="enemyLeft" class="enemy" style="
            width: 80px;
            height: 120px;
            background: rgba(0,0,0,0.5);
            border-radius: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            opacity: 0.4;
            transition: opacity 0.3s ease;
          ">
            <div style="width: 20px; height: 20px; background: #FFD700; border-radius: 50%; margin-bottom: 5px;"></div>
            <div style="width: 40px; height: 60px; background: #E63946; border-radius: 5px;"></div>
            <div style="position: relative; top: -30px; left: -20px; width: 30px; height: 10px; background: #E63946; transform: rotate(-45deg);"></div>
            <div style="position: relative; top: -40px; left: 15px; width: 20px; height: 10px; background: #E63946; transform: rotate(45deg);"></div>
          </div>
          
          <!-- 中间小人 - 扎马步 -->
          <div id="enemyMiddle" class="enemy" style="
            width: 80px;
            height: 120px;
            background: rgba(0,0,0,0.5);
            border-radius: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            opacity: 0.4;
            transition: opacity 0.3s ease;
          ">
            <div style="width: 20px; height: 20px; background: #FFD700; border-radius: 50%; margin-bottom: 5px;"></div>
            <div style="width: 40px; height: 60px; background: #E63946; border-radius: 5px;"></div>
            <div style="position: relative; top: -30px; left: -20px; width: 30px; height: 10px; background: #E63946;"></div>
            <div style="position: relative; top: -40px; left: 15px; width: 30px; height: 10px; background: #E63946;"></div>
          </div>
          
          <!-- 右侧小人 - 右冲拳 -->
          <div id="enemyRight" class="enemy" style="
            width: 80px;
            height: 120px;
            background: rgba(0,0,0,0.5);
            border-radius: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            opacity: 0.4;
            transition: opacity 0.3s ease;
          ">
            <div style="width: 20px; height: 20px; background: #FFD700; border-radius: 50%; margin-bottom: 5px;"></div>
            <div style="width: 40px; height: 60px; background: #E63946; border-radius: 5px;"></div>
            <div style="position: relative; top: -30px; left: -20px; width: 20px; height: 10px; background: #E63946; transform: rotate(45deg);"></div>
            <div style="position: relative; top: -40px; left: 15px; width: 30px; height: 10px; background: #E63946; transform: rotate(-45deg);"></div>
          </div>
        </div>
        
        <!-- 近处小熊 -->
        <div id="bear" style="
          position: absolute;
          bottom: 50px;
          left: 50%;
          width: 100px;
          height: 100px;
          transform: translateX(-50%);
          transition: left 0.3s ease;
        ">
          <!-- 小熊头部 -->
          <div style="
            width: 60px;
            height: 60px;
            background: #8B4513;
            border-radius: 50%;
            position: absolute;
            top: 10px;
            left: 20px;
          ">
            <div style="width: 10px; height: 10px; background: white; border-radius: 50%; position: absolute; top: 15px; left: 10px;"></div>
            <div style="width: 10px; height: 10px; background: white; border-radius: 50%; position: absolute; top: 15px; left: 40px;"></div>
            <div style="width: 20px; height: 5px; background: black; border-radius: 5px; position: absolute; top: 35px; left: 20px;"></div>
          </div>
          <!-- 小熊身体 -->
          <div style="
            width: 80px;
            height: 60px;
            background: #8B4513;
            border-radius: 40px;
            position: absolute;
            top: 50px;
            left: 10px;
          "></div>
        </div>
        
        <!-- 状态显示 -->
        <div style="
          position: absolute;
          top: 10px;
          left: 10px;
          background: rgba(0,0,0,0.7);
          color: white;
          padding: 10px;
          border-radius: 5px;
          font-family: Arial;
        ">
          <div>时间: <span id="staffTimer" style="font-size: 14px;">60</span>s</div>
          <div>成功次数: <span id="successCount" style="font-size: 14px;">0</span>/20</div>
        </div>
        
        <!-- 操作提示 -->
        <div style="
          position: absolute;
          top: 10px;
          right: 10px;
          background: rgba(0,0,0,0.7);
          color: white;
          padding: 10px;
          border-radius: 5px;
          font-family: Arial;
          text-align: center;
        ">
          <div>←: 向左移动</div>
          <div>→: 向右移动</div>
        </div>
        
        <!-- 拖动提示 -->
        <div style="
          position: absolute;
          bottom: 15px;
          left: 50%;
          transform: translateX(-50%);
          background: rgba(0,0,0,0.7);
          color: white;
          padding: 8px 15px;
          border-radius: 20px;
          font-family: Arial;
          font-size: 14px;
          white-space: nowrap;
        ">
          拖动小熊移动
        </div>
      </div>
      
      <div id="staffGameControls" style="
        text-align: center;
        margin-top: 20px;
      ">
        <button id="startStaffGame" class="action-btn" style="margin-right: 10px;">开始练习</button>
      </div>`;
    
    // 显示游戏界面
    const gameSection = document.querySelector('.game-section');
    if (gameSection) {
      // 移除可能存在的旧游戏内容
      const existingGameContent = gameSection.querySelector('#staffGame, #staffGameControls');
      if (existingGameContent) {
        existingGameContent.remove();
      }
      
      // 插入游戏HTML
      const tempContainer = document.createElement('div');
      tempContainer.innerHTML = gameHtml;
      
      // 找到"完成练习"按钮的位置，在其前面插入游戏内容
      const completeButton = gameSection.querySelector('.action-btn');
      gameSection.insertBefore(tempContainer.querySelector('#staffGame'), completeButton);
      gameSection.insertBefore(tempContainer.querySelector('#staffGameControls'), completeButton);
      
      // 隐藏原有的完成按钮
      if (completeButton) {
        completeButton.style.display = 'none';
      }
    }
    
    // 游戏逻辑变量
    let isPlaying = false;
    let timeLeft = 60;
    let successCount = 0;
    let currentEnemy = null;
    let enemyTimer = null;
    let gameTimer = null;
    let bearPosition = 1; // 0: 左, 1: 中, 2: 右
    let isDragging = false;
    let isEnemyHit = false; // 标记当前小人是否已被击中
    const enemyPositions = ['left', 'middle', 'right'];
    const gameContainerEl = document.getElementById('staffGame');
    
    // 获取DOM元素
    const timerEl = document.getElementById('staffTimer');
    const successCountEl = document.getElementById('successCount');
    const bearEl = document.getElementById('bear');
    const enemyLeftEl = document.getElementById('enemyLeft');
    const enemyMiddleEl = document.getElementById('enemyMiddle');
    const enemyRightEl = document.getElementById('enemyRight');
    const startGameBtn = document.getElementById('startStaffGame');
    
    // 开始游戏
    if (startGameBtn) {
      startGameBtn.addEventListener('click', function() {
        isPlaying = true;
        startGameBtn.disabled = true;
        successCount = 0;
        bearPosition = 1;
        updateBearPosition();
        
        // 开始倒计时
        gameTimer = setInterval(function() {
          timeLeft--;
          if (timerEl) {
            timerEl.textContent = timeLeft;
          }
          
          if (timeLeft <= 0) {
            endGame();
          }
        }, 1000);
        
        // 开始随机显示敌人
        enemyTimer = setInterval(showRandomEnemy, 2000);
        showRandomEnemy(); // 立即显示第一个敌人
      });
    }
    
    // 处理键盘输入
    document.addEventListener('keydown', handleStaffKeydown);
    
    // 设置鼠标/触摸拖动
    if (bearEl && gameContainerEl) {
      // 鼠标事件
      bearEl.addEventListener('mousedown', startDrag);
      document.addEventListener('mousemove', handleDrag);
      document.addEventListener('mouseup', endDrag);
      
      // 触摸事件
      bearEl.addEventListener('touchstart', startDrag, { passive: true });
      document.addEventListener('touchmove', handleDrag, { passive: true });
      document.addEventListener('touchend', endDrag, { passive: true });
    }
    
    // 拖动开始
    function startDrag(e) {
      if (!isPlaying) return;
      isDragging = true;
      // 更改鼠标样式
      if (bearEl) {
        bearEl.style.cursor = 'grabbing';
        // 防止文本选择
        bearEl.style.userSelect = 'none';
      }
    }
    
    // 拖动中
      function handleDrag(e) {
        if (!isDragging || !isPlaying || !gameContainerEl || !bearEl) return;
        
        let clientX;
        
        // 处理触摸和鼠标事件
        if (e.type.includes('touch')) {
          clientX = e.touches[0].clientX;
        } else {
          clientX = e.clientX;
        }
        
        // 获取游戏容器的位置和宽度
        const containerRect = gameContainerEl.getBoundingClientRect();
        const containerWidth = containerRect.width;
        
        // 计算相对于容器的位置比例
        const relativePosition = Math.max(0, Math.min(1, (clientX - containerRect.left) / containerWidth));
        
        // 更新小熊位置
        updateBearPositionByDrag(relativePosition);
        
        // 检查是否击中（只在当前小人未被击中时检查）
        if (!isEnemyHit) {
          checkHit();
        }
      }
    
    // 拖动结束
    function endDrag() {
      if (!isDragging) return;
      isDragging = false;
      // 恢复鼠标样式
      if (bearEl) {
        bearEl.style.cursor = 'grab';
        bearEl.style.userSelect = '';
      }
    }
    
    // 键盘处理函数
    function handleStaffKeydown(e) {
      if (!isPlaying) return;
      
      if (e.key === 'ArrowLeft') {
        moveBear('left');
      } else if (e.key === 'ArrowRight') {
        moveBear('right');
      }
    }
    
    // 移动小熊
      function moveBear(direction) {
        if (direction === 'left' && bearPosition > 0) {
          bearPosition--;
        } else if (direction === 'right' && bearPosition < 2) {
          bearPosition++;
        }
        updateBearPosition();
        // 检查是否击中（只在当前小人未被击中时检查）
        if (!isEnemyHit) {
          checkHit();
        }
      }
    
    // 通过拖动更新小熊位置
    function updateBearPositionByDrag(relativePosition) {
      // 根据相对位置确定小熊应该在哪个目标位置
      if (relativePosition < 0.33) {
        bearPosition = 0; // 左
      } else if (relativePosition < 0.66) {
        bearPosition = 1; // 中
      } else {
        bearPosition = 2; // 右
      }
      updateBearPosition();
    }
    
    // 更新小熊位置
    function updateBearPosition() {
      let leftPercentage = 25;
      if (bearPosition === 1) leftPercentage = 50;
      else if (bearPosition === 2) leftPercentage = 75;
      
      if (bearEl) {
        bearEl.style.left = leftPercentage + '%';
        // 设置鼠标样式为可抓取
        bearEl.style.cursor = 'grab';
      }
    }
    
    // 随机显示敌人
      function showRandomEnemy() {
        if (!isPlaying) return;
        
        // 隐藏所有敌人
        hideAllEnemies();
        
        // 重置击中标记
        isEnemyHit = false;
        
        // 随机选择一个敌人
        const randomIndex = Math.floor(Math.random() * 3);
        currentEnemy = enemyPositions[randomIndex];
        
        // 显示选中的敌人
        const enemyEl = document.getElementById('enemy' + capitalizeFirstLetter(currentEnemy));
        if (enemyEl) {
          enemyEl.style.opacity = '1';
          enemyEl.style.background = 'linear-gradient(135deg, #FFD700 0%, #FFA500 100%)';
        }
        
        // 2秒后自动隐藏
        setTimeout(function() {
          hideAllEnemies();
          currentEnemy = null;
          isEnemyHit = false;
        }, 1000);
      }
    
    // 隐藏所有敌人
      function hideAllEnemies() {
        [enemyLeftEl, enemyMiddleEl, enemyRightEl].forEach(enemy => {
          if (enemy) {
            enemy.style.opacity = '0.4';
            enemy.style.background = 'rgba(0,0,0,0.5)';
          }
        });
        // 重置击中标记
        isEnemyHit = false;
      }
    
    // 检查是否击中
      function checkHit() {
        if (!currentEnemy || isEnemyHit) return;
        
        // 确定当前敌人对应的位置索引
        let enemyIndex = 0;
        if (currentEnemy === 'middle') enemyIndex = 1;
        else if (currentEnemy === 'right') enemyIndex = 2;
        
        // 如果小熊位置与敌人位置一致，算成功
        if (bearPosition === enemyIndex) {
          // 标记为已击中，防止重复计算
          isEnemyHit = true;
          
          // 增加成功次数
          successCount++;
          if (successCountEl) {
            successCountEl.textContent = successCount;
          }
          
          // 显示成功效果
          const enemyEl = document.getElementById('enemy' + capitalizeFirstLetter(currentEnemy));
          if (enemyEl) {
            enemyEl.style.background = 'linear-gradient(135deg, #4CAF50 0%, #2E7D32 100%)';
          }
          
          // 立即隐藏当前敌人
          setTimeout(function() {
            hideAllEnemies();
            currentEnemy = null;
            isEnemyHit = false;
          }, 300);
        }
      }
    
    // 首字母大写
    function capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    }
    
    // 结束游戏
      function endGame() {
        const currentStory = getCurrentStory();
        clearInterval(gameTimer);
        clearInterval(enemyTimer);
        isPlaying = false;
        isDragging = false;
        isEnemyHit = false;
        hideAllEnemies();
        
        // 移除事件监听器
        document.removeEventListener('keydown', handleStaffKeydown);
        if (bearEl) {
          // 重置小熊样式
          bearEl.style.cursor = '';
          bearEl.style.userSelect = '';
        }
      
      // 计算奖励
      let rewards = [];
      let message = `游戏结束！你成功击中了 ${successCount} 次！\n`;
      
      if (successCount >= 20) {
        // 20次以上，奖励翻倍
        rewards = [{ type: 'strength', value: 16 }, { type: 'perseverance', value: 6 }];
        message += '表现优秀！获得双倍奖励：体魄+16、恒心+6';
      } else if (successCount >= 10) {
        // 10次以上，普通奖励
        rewards = [{ type: 'strength', value: 8 }, { type: 'perseverance', value: 3 }];
        message += '表现不错！获得奖励：体魄+8、恒心+3';
      } else {
        message += '继续努力！下一次会做得更好！';
      }
      
      // 应用奖励
      if (rewards.length > 0) {
        applyAttributeChanges(rewards);
        
        // 记录事件
        gameState.history.push({
          year: gameState.currentAge,
          event: currentStory.gameName,
          gains: rewards
        });
      }
      
      alert(message);
      // 进入下一年
      goToNextYear();
    }
  
  // 通用游戏完成逻辑
  const currentStory = getCurrentStory();
  // 应用游戏后的增益
  if (currentStory && currentStory.afterGameGains) {
    applyAttributeChanges(currentStory.afterGameGains);
  }
  // 进入下一年
  goToNextYear();
}

// 跳过游戏函数
const skipGame = () => {
  const currentStory = getCurrentStory()
  if (!currentStory || !currentStory.isGame) return;
  
  // 应用游戏后的增益
  if (currentStory && currentStory.afterGameGains) {
    applyAttributeChanges(currentStory.afterGameGains);
  }
  // 进入下一年
  goToNextYear();
}
</script>

<template>
  <div class="game-container">
    <!-- 顶部导航 -->
    <header class="game-header">
      <div class="header-content">
        <h1>武术传承</h1>
      </div>
    </header>
    
    <div class="game-body">
      <!-- 左侧面板 -->
      <aside class="left-panel">
        <!-- 角色信息 -->
        <div class="character-section">
          <div class="avatar-container">
            <div class="avatar">{{ character.avatar }}</div>
            <div class="character-name">{{ character.name }}</div>
          </div>
          
          <!-- 功能分类 -->
          <div class="function-section">
            <div class="section-title">秘籍</div>
          </div>
          
          <div class="function-section">
            <div class="section-title">兵器</div>
          </div>
          
          <!-- 功能按钮网格 -->
          <div class="action-buttons">
            <button class="action-btn" @click="toggleTaskByType('委托任务')">委托任务</button>
            <button class="action-btn" @click="toggleTaskByType('生活娱乐')">生活娱乐</button>
            <button class="action-btn" @click="toggleTaskByType('社区活动')">社区活动</button>
            <button class="action-btn" @click="toggleLibrary">藏书研修</button>
            <button class="action-btn" @click="showRelationshipMenu = true">人际互动</button>
            <button class="action-btn" @click="showArenaMenu = true">擂台竞技</button>
          </div>
          
        </div>
        </aside>
        
        <!-- 关系系统菜单 -->
        <div v-if="showRelationshipMenu" class="modal-overlay" @click="showRelationshipMenu = false">
          <div class="modal-content" @click.stop>
            <div class="modal-header">
              <h3>人际互动</h3>
              <button class="modal-close-btn" @click="showRelationshipMenu = false">×</button>
            </div>
            <div class="modal-body">
              <div class="relationship-menu">
                <p>今天你想去拜访哪位人物？</p>
                <div class="relationship-options">
                  <button class="relationship-btn" @click="{visitCharacter('师父'); showRelationshipMenu = false}">师父（行动点-1）</button>
                  <button class="relationship-btn" @click="{visitCharacter('大师兄'); showRelationshipMenu = false}">大师兄（行动点-1）</button>
                  <button class="relationship-btn" @click="{visitCharacter('大师姐'); showRelationshipMenu = false}">大师姐（行动点-1）</button>
                  <button class="relationship-btn" @click="{visitCharacter('小师弟'); showRelationshipMenu = false}">小师弟（行动点-1）</button>
                  <button class="relationship-btn" @click="{visitCharacter('小师妹'); showRelationshipMenu = false}">小师妹（行动点-1）</button>
                  <button class="relationship-btn" @click="{visitCharacter('藏书阁馆长'); showRelationshipMenu = false}">藏书阁馆长（行动点-1）</button>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <!-- 擂台比武菜单 -->
        <div v-if="showArenaMenu" class="modal-overlay" @click="showArenaMenu = false">
          <div class="modal-content" @click.stop>
            <div class="modal-header">
              <h3>擂台竞技</h3>
              <button class="modal-close-btn" @click="showArenaMenu = false">×</button>
            </div>
            <div class="modal-body">
              <div class="arena-menu">
                <div class="arena-options">
                  <button class="arena-btn" @click="{enterArena('beginner'); showArenaMenu = false}">初级擂台（行动点-1）</button>
                  <button class="arena-btn" @click="{enterArena('intermediate'); showArenaMenu = false}">中级擂台（行动点-1）</button>
                  <button class="arena-btn" @click="{enterArena('advanced'); showArenaMenu = false}">高级擂台（行动点-1）</button>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <!-- 通用事件弹窗 -->
        <div v-if="gameState.currentEvent" class="modal-overlay" @click="gameState.currentEvent = null">
          <div class="modal-content" @click.stop>
            <div class="modal-header">
              <h3>{{ gameState.currentEvent.title }}</h3>
              <button class="modal-close-btn" @click="gameState.currentEvent = null">×</button>
            </div>
            <div class="modal-body">
              <p class="event-description">{{ gameState.currentEvent.description }}</p>
              <div v-if="gameState.currentEvent.details" class="event-details">
                <ul>
                  <li v-for="(detail, index) in gameState.currentEvent.details" :key="index">{{ detail }}</li>
                </ul>
              </div>
              <div class="event-choices">
                <button 
                  v-for="(choice, index) in gameState.currentEvent.choices" 
                  :key="index"
                  class="choice-btn"
                  @click="choice.action()"
                >
                  {{ choice.text }}
                </button>
              </div>
            </div>
          </div>
        </div>
        
        <!-- 任务模态框 -->
        <div v-if="gameState.showTasks" class="modal-overlay" @click="toggleTasks">
          <div class="modal-content" @click.stop>
            <div class="modal-header">
              <h3>{{ gameState.currentTaskType || '任务中心' }}</h3>
              <button class="modal-close-btn" @click="toggleTasks">×</button>
            </div>
            
            <!-- 内部滚动容器 -->
            <div class="modal-body">
              <!-- 任务结果显示 -->
              <div v-if="gameState.taskResult" class="modal-task-result">
                <div class="result-message">任务完成！</div>
                <div v-if="gameState.taskResult.choiceResult" class="choice-result">
                  <p>{{ gameState.taskResult.choiceResult }}</p>
                </div>
                <div v-if="gameState.taskResult.lifeLesson" class="life-lesson">
                  <h5>生活启示：</h5>
                  <p>{{ gameState.taskResult.lifeLesson }}</p>
                </div>
                <div class="result-gains">
                  <div v-for="gain in gameState.taskResult.gains" :key="gain.type" class="gain-item">
                    {{ getAttributeName(gain.type) }} {{ gain.value > 0 ? '+' : '' }}{{ gain.value }}
                  </div>
                </div>
                <button class="close-result-btn" @click="gameState.taskResult = null">关闭</button>
              </div>
              
              <!-- 当前选中任务 -->
              <div v-if="gameState.currentTask" class="modal-current-task">
                <h4>{{ gameState.currentTask.name }}</h4>
                <p class="task-description">{{ gameState.currentTask.description }}</p>
                
                <!-- 场景和环境描述 -->
                <div v-if="gameState.currentTask.scene" class="task-scene">
                  <h5>场景：</h5>
                  <p>{{ gameState.currentTask.scene }}</p>
                </div>
                <div v-if="gameState.currentTask.environment" class="task-environment">
                  <h5>环境：</h5>
                  <p>{{ gameState.currentTask.environment }}</p>
                </div>
                
                <!-- 任务详情 -->
                <div v-if="gameState.currentTask.details" class="task-details">
                  <h5>任务详情：</h5>
                  <ul>
                    <li v-for="(detail, index) in gameState.currentTask.details" :key="index">{{ detail }}</li>
                  </ul>
                </div>
                
                <!-- 视频内容提示 -->
                <div v-if="gameState.currentTask.videoContent" class="video-content-hint">
                  <div class="hint-icon">🎬</div>
                  <p>此任务包含视频内容</p>
                </div>
                
                <!-- 测验提示 -->
                <div v-if="gameState.currentTask.quiz" class="quiz-hint">
                  <div class="hint-icon">📝</div>
                  <p>此任务包含知识测验</p>
                </div>
                
                <!-- 小游戏提示 -->
                <div v-if="gameState.currentTask.miniGame" class="mini-game-hint">
                  <div class="hint-icon">🎮</div>
                  <p>小游戏：{{ gameState.currentTask.miniGame }}</p>
                </div>
                
                <!-- 任务选项 -->
                <div v-if="gameState.currentTask.choices && !gameState.selectedTaskChoice" class="task-choices">
                  <h5>请选择：</h5>
                  <div class="choices-list">
                    <button 
                      v-for="(choice, index) in gameState.currentTask.choices" 
                      :key="index" 
                      class="choice-btn"
                      :class="{ active: false }"
                      @click="selectTaskChoice(choice)"
                    >
                      {{ choice.text }}
                    </button>
                  </div>
                </div>
                
                <!-- 已选选项 -->
                <div v-if="gameState.selectedTaskChoice" class="selected-choice">
                  <h5>已选择：</h5>
                  <p>{{ gameState.selectedTaskChoice.text }}</p>
                </div>
                
                <!-- 子任务列表 -->
                <div v-if="gameState.currentTask.subtasks && !gameState.selectedSubtask" class="subtasks-list">
                  <h5>子任务：</h5>
                  <div 
                    v-for="(subtask, index) in gameState.currentTask.subtasks" 
                    :key="index" 
                    class="subtask-item"
                    @click="selectSubtask(subtask)"
                  >
                    <h6>{{ subtask.name }}</h6>
                    <p>{{ subtask.interaction }}</p>
                  </div>
                </div>
                
                <!-- 当前子任务 -->
                <div v-if="gameState.selectedSubtask" class="current-subtask">
                  <h5>{{ gameState.selectedSubtask.name }}</h5>
                  <p>{{ gameState.selectedSubtask.interaction }}</p>
                  
                  <!-- 子任务选项 -->
                  <div v-if="gameState.selectedSubtask.options" class="subtask-options">
                    <button 
                      v-for="(option, index) in gameState.selectedSubtask.options" 
                      :key="index" 
                      class="option-btn"
                      @click="completeSubtask(option)"
                    >
                      {{ option.text }}
                    </button>
                  </div>
                  
                  <!-- 子任务小游戏 -->
                  <div v-if="gameState.selectedSubtask.miniGame" class="subtask-mini-game">
                    <p>{{ gameState.selectedSubtask.miniGame }}</p>
                    <p><small>成功条件：{{ gameState.selectedSubtask.successCondition }}</small></p>
                  </div>
                </div>
                
                <!-- 子任务结果 -->
                <div v-if="gameState.subtaskResults && gameState.subtaskResults.length > 0" class="subtask-results">
                  <h5>子任务完成情况：</h5>
                  <ul>
                    <li v-for="(result, index) in gameState.subtaskResults" :key="index">
                      <span :class="{ success: result.success, fail: !result.success }">
                        {{ result.subtaskName }}：{{ result.result }}
                      </span>
                    </li>
                  </ul>
                </div>
                
                <!-- 任务奖励 -->
                <div class="task-gains">
                  <h5>任务奖励：</h5>
                  <div class="gains-display">
                    <span v-for="gain in gameState.currentTask.gains" :key="gain.type" class="gain-tag">
                      {{ getAttributeName(gain.type) }} {{ gain.value > 0 ? '+' : '' }}{{ gain.value }}
                    </span>
                  </div>
                </div>
                
                <!-- 任务操作按钮 -->
                <div class="task-actions">
                  <button class="complete-task-btn" @click="completeTask">
                    {{ gameState.currentTask.choices ? '确认选择' : '完成任务' }}
                  </button>
                  <button v-if="gameState.selectedSubtask" class="back-btn" @click="gameState.selectedSubtask = null">
                    返回子任务列表
                  </button>
                  <button class="cancel-task-btn" @click="cancelTask">取消任务</button>
                </div>
              </div>
              
              <!-- 任务列表 -->
              <div v-else class="modal-tasks-list">
                <div v-for="task in getAvailableTasks()" :key="task.id" class="task-item" @click="selectTask(task)">
                  <h4>{{ task.name }}</h4>
                  <p class="task-type">{{ task.type }}</p>
                  <p class="task-brief">{{ task.description.substring(0, 50) }}...</p>
                  <div class="task-preview-gains">
                    <span v-for="gain in task.gains" :key="gain.type" class="gain-preview">
                      {{ getAttributeName(gain.type) }}{{ gain.value > 0 ? '+' : '' }}{{ gain.value }}
                    </span>
                  </div>
                </div>
                <div v-if="getAvailableTasks().length === 0" class="no-tasks">
                  当前没有可接取的任务
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <!-- 藏书阁模态框 -->
        <div v-if="gameState.showLibrary" class="modal-overlay" @click="toggleLibrary">
          <div class="modal-content" @click.stop>
            <div class="modal-header">
              <h3>藏书研修</h3>
              <button class="modal-close-btn" @click="toggleLibrary">×</button>
            </div>
            
            <!-- 内部滚动容器 -->
            <div class="modal-body">
              <!-- 藏书阁结果显示 -->
              <div v-if="gameState.libraryResult" class="modal-task-result">
                <div class="result-message">{{ gameState.libraryResult.content }}</div>
                <div class="result-gains" v-if="gameState.libraryResult.gains && gameState.libraryResult.gains.length > 0">
                  <div v-for="gain in gameState.libraryResult.gains" :key="gain.type" class="gain-item">
                    {{ getAttributeName(gain.type) }} {{ gain.value > 0 ? '+' : '' }}{{ gain.value }}
                  </div>
                </div>
                <button class="complete-task-btn" style="margin-top: 15px;" @click="cancelLibraryAction">继续参悟</button>
              </div>
              
              <!-- 当前选中的藏书阁动作 -->
              <div v-else-if="gameState.currentLibraryAction" class="modal-current-task">
                <h4>{{ gameState.currentLibraryAction.text }}</h4>
                <p class="task-description">{{ gameState.currentLibraryAction.description }}</p>
                
                <!-- 流派选择 -->
                <div v-if="gameState.currentLibraryAction.type === '了解武德'" class="style-choices">
                  <h5>选择流派：</h5>
                  <div class="style-buttons">
                    <button 
                      v-for="style in libraryContent.styles" 
                      :key="style.name"
                      class="action-btn style-btn"
                      @click="selectLibraryAction({...gameState.currentLibraryAction, subChoice: { type: 'style', value: style.name, gains: style.gains }})"
                    >
                      {{ style.name }}
                    </button>
                  </div>
                </div>
                
                <!-- 武技类型选择 -->
                <div v-else-if="gameState.currentLibraryAction.type === '参悟武技'" class="skill-type-choices">
                  <h5>选择武技类型：</h5>
                  <div class="skill-type-buttons">
                    <button class="action-btn" @click="selectLibraryAction({...gameState.currentLibraryAction, subChoice: { type: 'skillType', value: '拳法' }})">{{ gameState.currentLibraryAction.style ? gameState.currentLibraryAction.style + '拳法' : '拳法' }}</button>
                    <button class="action-btn" @click="selectLibraryAction({...gameState.currentLibraryAction, subChoice: { type: 'skillType', value: '腿法' }})">{{ gameState.currentLibraryAction.style ? gameState.currentLibraryAction.style + '腿法' : '腿法' }}</button>
                    <button class="action-btn" @click="selectLibraryAction({...gameState.currentLibraryAction, subChoice: { type: 'skillType', value: '武器' }})">{{ gameState.currentLibraryAction.style ? gameState.currentLibraryAction.style + '武器' : '武器' }}</button>
                  </div>
                </div>
                
                <!-- 具体技能选择 -->
                <div v-else-if="gameState.currentLibraryAction.subChoice && gameState.currentLibraryAction.subChoice.type === 'skillType'" class="skill-choices">
                  <h5>选择技能：</h5>
                  <div class="skill-buttons">
                    <button 
                      v-for="skill in getAvailableSkills(gameState.currentLibraryAction.style, gameState.currentLibraryAction.subChoice.value)" 
                      :key="skill.name"
                      class="action-btn skill-btn"
                      @click="selectLibraryAction({...gameState.currentLibraryAction, subChoice: { 
                        type: 'skill', 
                        skill: skill.name, 
                        gains: skill.gains, 
                        proficiency: skill.proficiency 
                      }})"
                    >
                      {{ skill.name }}
                      <br>
                      <small>当前熟练度：{{ gameState.skillProficiency[skill.name] || 0 }}%</small>
                    </button>
                  </div>
                </div>
                
                <div class="task-actions">
                  <button class="cancel-task-btn" @click="cancelLibraryAction">返回</button>
                </div>
              </div>
              
              <!-- 藏书阁主界面 -->
              <div v-else class="library-main">
                <h4>来到藏书阁，你打算：</h4>
                <div class="library-options">
                  <div 
                    v-for="option in libraryContent.mainOptions" 
                    :key="option.type"
                    class="library-option-item"
                    @click="selectLibraryAction(option)"
                  >
                    <h5>{{ option.text }}</h5>
                    <p>{{ option.description }}</p>
                    <div class="cost-info">行动点 -1</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      
      <!-- 主内容区 -->
      <main class="main-content">
        <!-- 属性面板 -->
        <div class="stats-container">
          <div class="action-points-container">
            <div class="action-points-label">剩余行为点</div>
            <div class="action-points-value">{{ actionPoints }}</div>
          </div>
          <div class="attributes-grid">
            <div class="attribute-item">
              <span class="attribute-name">体魄</span>
              <span class="attribute-value">{{ strengthValue }}</span>
            </div>
            <div class="attribute-item">
              <span class="attribute-name">武德</span>
              <span class="attribute-value">{{ martialArtsValue }}</span>
            </div>
            <div class="attribute-item">
              <span class="attribute-name">人品</span>
              <span class="attribute-value">{{ personalityValue }}</span>
            </div>
            <div class="attribute-item">
              <span class="attribute-name">心情</span>
              <span class="attribute-value">{{ moodValue }}</span>
            </div>
            <div class="attribute-item">
              <span class="attribute-name">恒心</span>
              <span class="attribute-value">{{ perseveranceValue }}</span>
            </div>
            <div class="attribute-item">
              <span class="attribute-name">声望</span>
              <span class="attribute-value">{{ reputationValue }}</span>
            </div>
          </div>
        </div>
        
        <!-- 故事内容区 -->
        <div class="story-container">
          <h2>{{ gameState.currentAge }}岁</h2>
          <div class="story-content">
            <p>{{ getCurrentStory()?.content }}</p>
            
            <!-- 特殊事件显示 -->
            <div v-if="getCurrentStory()?.specialEvent" class="special-event">
              <strong>{{ getCurrentStory().specialEvent }}</strong>
            </div>
            
            <!-- 游戏部分 -->
            <div v-if="getCurrentStory()?.isGame && !gameState.showChoiceResult" class="game-section">
              <h3>{{ getCurrentStory().gameName }}</h3>
              <p v-if="getCurrentStory().gameDescription" style="white-space: pre-wrap;">{{ getCurrentStory().gameDescription }}</p>
              <div v-if="getCurrentStory().specialSkill" class="special-skill">
                解锁技能: {{ getCurrentStory().specialSkill }}
              </div>
              <button @click="completeGame" class="action-btn">完成练习</button>
              <button @click="skipGame" class="action-btn" style="margin-left: 10px; background-color: #f39c12; color: white;">跳过游戏</button>
            </div>
            
            <!-- 选择部分 -->
            <div v-if="getCurrentStory()?.choices && !gameState.showChoiceResult" class="choices">
              <h3>请选择你的行动:</h3>
              <div class="action-buttons" style="grid-template-columns: 1fr; gap: 10px; margin-top: 15px;">
                <button 
                  v-for="(choice, index) in getCurrentStory().choices" 
                  :key="index"
                  @click="selectChoice(choice)"
                  class="action-btn"
                >
                  {{ choice.text }}
                </button>
              </div>
            </div>
            
            <!-- 选择结果 -->
            <div v-if="gameState.showChoiceResult && gameState.selectedChoice" class="choice-result">
              <p>{{ gameState.selectedChoice.content }}</p>
              <div v-if="gameState.selectedChoice.gains && gameState.selectedChoice.gains.length > 0" class="gains">
                <span v-for="(gain, index) in gameState.selectedChoice.gains" :key="index" class="gain-item green-text">
                  ({{ getAttributeName(gain.type) }} {{ gain.value > 0 ? '+' : '' }}{{ gain.value }})
                </span>
              </div>
              <div v-if="gameState.selectedChoice.restrictions && gameState.selectedChoice.restrictions.length > 0" class="restrictions">
                <span v-for="(restriction, index) in gameState.selectedChoice.restrictions" :key="index" class="gain-item" style="color: #8b0000;">
                  {{ restriction }}
                </span>
              </div>
            </div>
            
            <!-- 普通增益显示 -->
            <div v-if="!getCurrentStory()?.choices && getCurrentStory()?.gains && !gameState.showChoiceResult" class="gains">
              <span v-for="(gain, index) in getCurrentStory().gains" :key="index" class="gain-item green-text">
                ({{ getAttributeName(gain.type) }} {{ gain.value > 0 ? '+' : '' }}{{ gain.value }})
              </span>
            </div>
          </div>
        </div>
        
        <!-- 继续按钮 - 用于选择结果展示后 -->
        <button 
          v-if="gameState.showChoiceResult"
          class="next-year-button" 
          @click="continueToNextStep"
        >
          继续
          <span class="arrow-icon">▶</span>
        </button>
        
        <!-- 下一年按钮 - 根据游戏状态显示 -->
        <button 
          v-else-if="!getCurrentStory()?.choices && !getCurrentStory()?.isGame"
          class="next-year-button" 
          @click="goToNextYear"
        >
          下一年
          <span class="arrow-icon">▶</span>
        </button>
      </main>
    </div>
    
    <!-- 背景装饰 -->
    <div class="background-decoration"></div>
  </div>
</template>

<style>
/* 全局样式重置 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  background-color: #f0f0f0;
  overflow: hidden;
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

#app {
  height: 100%;
  width: 100%;
}
</style>

<style scoped>
/* 游戏容器 */
.game-container {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  background: linear-gradient(135deg, #f5f5f5 0%, #e8e8e8 100%);
  position: relative;
  overflow: hidden;
}

/* 顶部导航 */
.game-header {
  height: 60px;
  background: linear-gradient(90deg, #8b0000, #a00000);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
  z-index: 100;
}

.header-content h1 {
  font-size: 24px;
  font-weight: bold;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
}

/* 主体内容 */
.game-body {
  flex: 1;
  display: flex;
  overflow: hidden;
}

/* 左侧面板 */
.left-panel {
  width: 220px;
  background-color: rgba(255, 255, 255, 0.95);
  border-right: 1px solid #e0e0e0;
  display: flex;
  flex-direction: column;
  padding: 20px 15px;
}

.character-section {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* 角色信息 */
.avatar-container {
  text-align: center;
  padding-bottom: 20px;
  border-bottom: 2px solid #e0e0e0;
}

.avatar {
  font-size: 64px;
  width: 100px;
  height: 100px;
  margin: 0 auto 10px;
  background-color: #fff9e6;
  border: 2px solid #ffcc00;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.character-name {
  font-size: 18px;
  font-weight: bold;
  color: #333;
  margin-top: 8px;
}

/* 功能分类 */
.function-section {
  padding: 8px 0;
}

.section-title {
  font-size: 15px;
  font-weight: bold;
  color: #666;
  padding: 5px 0;
  text-align: center;
  background-color: #f5f5f5;
  border-radius: 4px;
  margin-bottom: 5px;
}

/* 功能按钮 */
.action-buttons {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  margin-top: auto;
}

.action-btn {
  padding: 12px 8px;
  background-color: #ffcc00;
  border: 1px solid #e6b800;
  border-radius: 4px;
  font-size: 13px;
  font-weight: bold;
  color: #333;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.action-btn:hover {
  background-color: #ffd633;
  transform: translateY(-1px);
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.15);
}

/* 任务模态框样式 */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2000;
}

.modal-content {
    background-color: #fff;
    border-radius: 12px;
    border: 2px solid #ffcc00;
    width: 90%;
    max-width: 500px;
    max-height: 80vh;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  }

.modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 25px;
    border-bottom: 2px solid #e0e0e0;
    background-color: #fff9e6;
    border-radius: 10px 10px 0 0;
    flex-shrink: 0;
  }
  
  /* 内部滚动容器样式 */
  .modal-body {
    max-height: calc(80vh - 80px);
    overflow-y: auto;
    -ms-overflow-style: none;  /* IE and Edge */
    scrollbar-width: none;     /* Firefox */
  }
  
  /* 隐藏Chrome, Safari等的滚动条 */
  .modal-body::-webkit-scrollbar {
    display: none;
  }
  
  .modal-header h3 {
  margin: 0;
  color: #333;
  font-size: 20px;
  font-weight: bold;
}

.modal-close-btn {
  background: none;
  border: none;
  font-size: 28px;
  cursor: pointer;
  color: #666;
  padding: 0;
  width: 35px;
  height: 35px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: all 0.2s ease;
}

.modal-close-btn:hover {
  background-color: #f0f0f0;
  color: #ffcc00;
}

.modal-task-result,
.modal-current-task,
.modal-tasks-list {
  padding: 25px;
}

.modal-task-result {
  background-color: #e6f7ff;
  border: 1px solid #91d5ff;
  border-radius: 8px;
  margin-bottom: 20px;
}

.result-message {
  font-weight: bold;
  color: #0050b3;
  margin-bottom: 12px;
  font-size: 18px;
}

.result-gains {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.gain-item {
  background-color: #fff;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 14px;
  color: #0050b3;
  border: 1px solid #91d5ff;
}

.modal-current-task h4 {
  margin-top: 0;
  color: #389e0d;
  font-size: 22px;
  margin-bottom: 15px;
}

.task-description {
  color: #555;
  margin-bottom: 20px;
  line-height: 1.6;
  font-size: 15px;
}

.task-details {
  margin-bottom: 20px;
  background-color: #fafafa;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #e8e8e8;
}

.task-details h5,
.task-gains h5 {
  margin-top: 0;
  margin-bottom: 12px;
  color: #333;
  font-size: 16px;
  font-weight: bold;
}

.task-details ul {
  margin: 0;
  padding-left: 20px;
}

.task-details li {
  color: #666;
  margin-bottom: 8px;
  line-height: 1.5;
  font-size: 14px;
}

.task-gains {
  margin-bottom: 25px;
  background-color: #fafafa;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #e8e8e8;
}

.gains-display {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.gain-tag {
  background-color: #f6ffed;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 14px;
  color: #389e0d;
  border: 1px solid #b7eb8f;
  font-weight: 500;
}

.task-actions {
  display: flex;
  gap: 15px;
  margin-top: 25px;
  justify-content: center;
  flex-wrap: wrap;
}

/* 任务结果样式扩展 */
.choice-result {
  background-color: #fff;
  padding: 15px;
  border-radius: 6px;
  margin: 15px 0;
  border: 1px solid #e8e8e8;
  line-height: 1.6;
  color: #555;
}

.life-lesson {
  background-color: #fff7e6;
  padding: 15px;
  border-radius: 6px;
  margin: 15px 0;
  border: 1px solid #ffd591;
}

.life-lesson h5 {
  color: #fa8c16;
  margin-top: 0;
  margin-bottom: 10px;
}

.life-lesson p {
  margin: 0;
  line-height: 1.6;
  color: #663f11;
}

.close-result-btn {
  background-color: #1890ff;
  color: white;
  border: none;
  padding: 8px 20px;
  border-radius: 6px;
  cursor: pointer;
  margin-top: 15px;
  font-size: 14px;
}

.close-result-btn:hover {
  background-color: #40a9ff;
}

/* 场景和环境样式 */
.task-scene,
.task-environment {
  margin-bottom: 15px;
  background-color: #f0f5ff;
  padding: 15px;
  border-radius: 6px;
  border: 1px solid #adc6ff;
}

.task-scene h5,
.task-environment h5 {
  color: #1890ff;
  margin-top: 0;
  margin-bottom: 8px;
  font-size: 15px;
}

/* 提示样式 */
.video-content-hint,
.quiz-hint,
.mini-game-hint {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px;
  border-radius: 6px;
  margin: 15px 0;
  font-size: 14px;
}

.video-content-hint {
  background-color: #fff1f0;
  border: 1px solid #ffccc7;
  color: #cf1322;
}

.quiz-hint {
  background-color: #f6ffed;
  border: 1px solid #b7eb8f;
  color: #389e0d;
}

.mini-game-hint {
  background-color: #f9f0ff;
  border: 1px solid #d3adf7;
  color: #722ed1;
}

.hint-icon {
  font-size: 20px;
}

/* 选项样式 */
.task-choices {
  margin: 20px 0;
}

.task-choices h5 {
  color: #333;
  margin-bottom: 15px;
}

.choices-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.choice-btn {
  padding: 12px 20px;
  border: 2px solid #d9d9d9;
  border-radius: 6px;
  background-color: white;
  cursor: pointer;
  text-align: left;
  transition: all 0.3s;
  font-size: 14px;
  line-height: 1.5;
}

.choice-btn:hover {
  border-color: #40a9ff;
  color: #1890ff;
}

.choice-btn.active {
  border-color: #1890ff;
  background-color: #e6f7ff;
  color: #1890ff;
}

.selected-choice {
  background-color: #e6f7ff;
  padding: 15px;
  border-radius: 6px;
  border: 1px solid #91d5ff;
  margin: 15px 0;
}

.selected-choice h5 {
  color: #0050b3;
  margin-top: 0;
  margin-bottom: 8px;
}

/* 子任务样式 */
.subtasks-list {
  margin: 20px 0;
}

.subtasks-list h5 {
  color: #333;
  margin-bottom: 15px;
}

.subtask-item {
  padding: 15px;
  border: 1px solid #d9d9d9;
  border-radius: 6px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: all 0.3s;
  background-color: white;
}

.subtask-item:hover {
  border-color: #40a9ff;
  background-color: #f5f5f5;
}

.subtask-item h6 {
  margin: 0 0 8px 0;
  color: #1890ff;
  font-size: 15px;
}

.subtask-item p {
  margin: 0;
  color: #666;
  font-size: 14px;
}

.current-subtask {
  background-color: #f5f5f5;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #d9d9d9;
  margin: 20px 0;
}

.current-subtask h5 {
  color: #1890ff;
  margin-top: 0;
  margin-bottom: 12px;
  font-size: 16px;
}

.subtask-options {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin: 15px 0;
}

.option-btn {
  padding: 10px 16px;
  border: 1px solid #d9d9d9;
  border-radius: 6px;
  background-color: white;
  cursor: pointer;
  text-align: left;
  transition: all 0.3s;
  font-size: 14px;
}

.option-btn:hover {
  border-color: #52c41a;
  color: #52c41a;
}

.subtask-mini-game {
  background-color: #f6ffed;
  padding: 15px;
  border-radius: 6px;
  border: 1px dashed #b7eb8f;
  margin: 15px 0;
}

.subtask-mini-game p {
  margin: 8px 0;
}

/* 子任务结果样式 */
.subtask-results {
  background-color: #fafafa;
  padding: 15px;
  border-radius: 6px;
  margin: 20px 0;
  border: 1px solid #e8e8e8;
}

.subtask-results h5 {
  color: #333;
  margin-top: 0;
  margin-bottom: 12px;
}

.subtask-results ul {
  margin: 0;
  padding-left: 20px;
}

.subtask-results li {
  margin-bottom: 8px;
  line-height: 1.5;
}

.subtask-results .success {
  color: #389e0d;
  font-weight: 500;
}

.subtask-results .fail {
  color: #cf1322;
}

/* 返回按钮样式 */
.back-btn {
  padding: 8px 20px;
  border: 1px solid #d9d9d9;
  border-radius: 6px;
  background-color: white;
  cursor: pointer;
  transition: all 0.3s;
  font-size: 14px;
  color: #666;
}

.back-btn:hover {
  border-color: #1890ff;
  color: #1890ff;
}

.complete-task-btn,
.cancel-task-btn {
  padding: 12px 24px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 500;
  transition: all 0.3s ease;
  min-width: 120px;
}

.complete-task-btn {
  background-color: #52c41a;
  color: white;
}

.complete-task-btn:hover {
  background-color: #389e0d;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(82, 196, 26, 0.3);
}

.cancel-task-btn {
  background-color: #f5f5f5;
  color: #333;
  border: 1px solid #e8e8e8;
}

.cancel-task-btn:hover {
  background-color: #e8e8e8;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.modal-tasks-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.task-item {
  background-color: #fff;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  padding: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.task-item:hover {
  border-color: #1890ff;
  box-shadow: 0 4px 15px rgba(24, 144, 255, 0.15);
  transform: translateY(-3px);
}

.task-item h4 {
  margin: 0 0 10px 0;
  color: #333;
  font-size: 18px;
  font-weight: bold;
}

.task-type {
  margin: 0 0 10px 0;
  color: #1890ff;
  font-size: 13px;
  background-color: #e6f7ff;
  display: inline-block;
  padding: 4px 10px;
  border-radius: 4px;
  font-weight: 500;
}

.task-brief {
  margin: 10px 0;
  color: #666;
  font-size: 14px;
  line-height: 1.5;
}

.task-preview-gains {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 12px;
}

.gain-preview {
  background-color: #f0f0f0;
  padding: 4px 10px;
  border-radius: 4px;
  font-size: 13px;
  color: #666;
  font-weight: 500;
}

.no-tasks {
  text-align: center;
  color: #999;
  padding: 40px 20px;
  font-size: 16px;
  font-style: italic;
}

/* 藏书阁相关样式 */
.library-main h4 {
  margin-bottom: 20px;
  color: #333;
  text-align: center;
}

.library-options {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.library-option-item {
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  border: 2px solid #ddd;
  border-radius: 10px;
  padding: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  text-align: center;
}

.library-option-item:hover {
  transform: translateY(-3px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  border-color: #999;
}

.library-option-item h5 {
  margin: 0 0 10px 0;
  color: #333;
  font-size: 1.1em;
}

.library-option-item p {
  color: #666;
  margin-bottom: 10px;
  font-size: 0.95em;
}

.cost-info {
  background: #ffecd2;
  color: #d68c45;
  display: inline-block;
  padding: 5px 12px;
  border-radius: 15px;
  font-size: 0.9em;
  font-weight: bold;
}

/* 流派选择样式 */
.style-choices,
.skill-type-choices,
.skill-choices {
  margin: 20px 0;
}

.style-choices h5,
.skill-type-choices h5,
.skill-choices h5 {
  margin-bottom: 15px;
  color: #333;
  text-align: center;
}

.style-buttons,
.skill-type-buttons,
.skill-buttons {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.style-btn,
.skill-btn {
  background: linear-gradient(135deg, #fff3e0 0%, #ffcc80 100%);
  border: 2px solid #ffa726;
  border-radius: 8px;
  padding: 15px;
  font-size: 1em;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
}

.style-btn:hover,
.skill-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 10px rgba(255, 167, 38, 0.3);
  background: linear-gradient(135deg, #ffe0b2 0%, #ffb74d 100%);
}

.skill-btn small {
  display: block;
  margin-top: 5px;
  font-size: 0.8em;
  font-weight: normal;
  color: #666;
}

/* 主内容区 */
.main-content {
  flex: 1;
  padding: 20px;
  position: relative;
  overflow-y: auto;
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;     /* Firefox */
  background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><path d="M50,10 C60,15 70,25 75,35 C80,45 75,55 70,65 C65,75 55,85 45,90 C35,95 25,90 20,80 C15,70 15,55 20,45 C25,35 35,20 50,10 Z" fill="%23666" opacity="0.08"/><path d="M50,15 C60,20 65,25 70,35 C73,45 70,55 65,60 C60,65 55,68 50,65 C45,62 40,55 35,50 C30,45 30,35 35,25 C40,18 45,12 50,15 Z" fill="%23333" opacity="0.05"/></svg>');
  background-size: 300px;
  background-position: center bottom;
  background-repeat: no-repeat;
}

/* 隐藏主内容区的滚动条（Chrome, Safari等） */
.main-content::-webkit-scrollbar {
  display: none;
}

/* 属性面板 */
.stats-container {
  background-color: rgba(255, 255, 255, 0.95);
  border-radius: 8px;
  padding: 15px;
  margin-bottom: 20px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
}

/* 剩余行为点 */
.action-points-container {
  background: linear-gradient(135deg, #e67e22, #d35400);
  color: white;
  border-radius: 6px;
  padding: 6px 12px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  max-width: 200px;
}

.action-points-label {
  font-size: 14px;
  font-weight: bold;
}

.action-points-value {
  font-size: 18px;
  font-weight: bold;
  background-color: rgba(255, 255, 255, 0.2);
  padding: 3px 10px;
  border-radius: 4px;
  min-width: 40px;
  text-align: center;
}

/* 属性网格 */
.attributes-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
}

.attribute-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #f9f9f9;
  border-radius: 6px;
  border-left: 3px solid #ffcc00;
}

.attribute-name {
  font-size: 14px;
  color: #666;
  font-weight: 500;
}

.attribute-value {
  font-size: 18px;
  font-weight: bold;
  color: #333;
}

/* 故事容器 */
.story-container {
  background-color: rgba(255, 255, 255, 0.95);
  border-radius: 8px;
  padding: 20px;
  min-height: 300px;
  margin-bottom: 60px;
}

.story-container h2 {
  font-size: 22px;
  color: #333;
  margin-bottom: 15px;
  padding-bottom: 8px;
  border-bottom: 2px solid #ffcc00;
}

.story-content {
  font-size: 16px;
  line-height: 1.6;
  color: #333;
  margin-bottom: 20px;
}

.story-content p {
  font-size: 15px;
  line-height: 1.7;
  color: #444;
  text-align: justify;
}

/* 增益显示 */
.gains {
  margin-top: 15px;
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.gain-item {
  background-color: #e8f5e9;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 500;
}

.green-text {
  color: #2e7d32;
}

/* 特殊事件样式 */
.special-event {
  background-color: #fff9e6;
  border-left: 4px solid #ffcc00;
  padding: 10px;
  margin: 15px 0;
  border-radius: 4px;
  color: #8b0000;
}

/* 游戏部分样式 */
.game-section {
  background-color: #e8f5e9;
  border-radius: 8px;
  padding: 15px;
  margin: 15px 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.game-section h3 {
  color: #2e7d32;
  margin-bottom: 10px;
}

.special-skill {
  background-color: #c8e6c9;
  padding: 8px 12px;
  border-radius: 4px;
  margin: 10px 0;
  color: #1b5e20;
  font-weight: bold;
}

/* 选择部分样式 */
.choices h3 {
  color: #333;
  margin-bottom: 10px;
}

/* 选择结果样式 */
.choice-result {
  background-color: #f5f5f5;
  border-radius: 8px;
  padding: 15px;
  margin-top: 15px;
  border: 1px solid #e0e0e0;
}

/* 限制条件样式 */
.restrictions {
  margin-top: 10px;
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

/* 关系和擂台样式 */
.relationship-btn,
.arena-btn {
  width: 100%;
  padding: 12px;
  margin: 8px 0;
  border: none;
  background-color: #8B4513;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.relationship-btn:hover,
.arena-btn:hover {
  background-color: #A0522D;
}

.relationship-menu,
.arena-menu {
  padding: 20px;
}

.relationship-options,
.arena-options {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.relationship-menu p,
.arena-menu p {
  font-size: 16px;
  color: #333;
  margin-bottom: 15px;
  font-weight: 500;
}

.event-description {
  margin-bottom: 20px;
  line-height: 1.6;
  font-size: 15px;
  color: #444;
}

.event-details {
  margin-bottom: 20px;
  background-color: #f9f9f9;
  padding: 15px;
  border-radius: 6px;
  border-left: 3px solid #ffcc00;
}

.event-details ul {
  list-style: none;
  padding-left: 0;
}

.event-details li {
  padding: 5px 0;
  border-bottom: 1px solid #eee;
  color: #666;
}

.event-choices {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.event-choices .choice-btn {
  width: 100%;
  text-align: center;
}

/* 下一年按钮 */
.next-year-button {
  position: absolute;
  bottom: 30px;
  right: 30px;
  background-color: #ffcc00;
  color: #333;
  border: 1px solid #e6b800;
  border-radius: 6px;
  padding: 12px 20px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  box-shadow: 0 4px 12px rgba(255, 204, 0, 0.3);
  transition: all 0.3s ease;
}

.next-year-button:hover {
  background-color: #ffd633;
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(255, 204, 0, 0.4);
}

.arrow-icon {
  font-size: 18px;
  font-weight: bold;
}

/* 背景装饰 */
.background-decoration {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 300px;
  background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320"><path fill="%23666" fill-opacity="0.08" d="M0,96L48,106.7C96,117,192,139,288,133.3C384,128,480,96,576,101.3C672,107,768,149,864,165.3C960,181,1056,171,1152,154.7C1248,139,1344,117,1392,106.7L1440,96L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z"></path></svg>');
  background-size: cover;
  background-position: bottom;
  pointer-events: none;
  z-index: -1;
}

/* 响应式设计 - 桌面端优化 */
@media (min-width: 1200px) {
  .attributes-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .main-content {
    padding: 30px;
  }
  
  .story-container {
    min-height: 350px;
  }
}

/* 响应式设计 - 平板端横屏 */
@media (min-width: 768px) and (max-width: 1199px) {
  .left-panel {
    width: 200px;
    padding: 15px 10px;
  }
  
  .avatar {
    font-size: 56px;
    width: 90px;
    height: 90px;
  }
  
  .attributes-grid {
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
  }
  
  .stats-container {
    padding: 12px;
  }
  
  .story-container {
    padding: 15px;
    min-height: 280px;
  }
}

/* 响应式设计 - 手机横屏 */
@media (max-width: 767px) and (orientation: landscape) {
  .game-body {
    flex-direction: column;
  }
  
  .left-panel {
    width: 100%;
    height: auto;
    max-height: 200px;
  }
  
  .attributes-grid {
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
  }
  
  .next-year-button {
    position: relative;
    bottom: auto;
    right: auto;
    margin-top: 20px;
    width: 100%;
    justify-content: center;
  }
}
</style>
