---
layout: post
title: "Data Structure: Graph DFS, BFS and Prim algorithm"
author: "Eddy Hu"
categories: journal
tags: [algorithm, data structure, java]
image: graph.jpeg
---


**Graph**

    import java.util.LinkedList;

    public class Graph {

	private int vertexSize;
	private int[] vertices;
	private int[][] matrix;
	private static final int MAX_WEIGHT = 1000;
	private boolean[] isVisited;
	
	public Graph(int vertexSize) {
		this.vertexSize = vertexSize;
		matrix= new int[vertexSize][vertexSize];
		vertices = new int[vertexSize];
		isVisited = new boolean[vertexSize];
		for	(int i =0; i<vertexSize; i++) {
			vertices[i]=i;
		}
	}
	// get out degree
	public int getOutDegree(int index) {
		int degree = 0;
		for(int i =0; i<matrix[index].length;i++) {
			int weight = matrix[index][i];
			if(weight!=0&&weight <MAX_WEIGHT) {
				degree++;
			}
		}
		return degree;
	}
	
	//get in degree
	public int getInDegree(int index) {
		int degree = 0;
		for(int i =0; i<matrix.length;i++) {
			int weight = matrix[i][index];
			if(weight!=0&&weight <MAX_WEIGHT) {
				degree++;
			}
		}
		return degree;
	}
	
	//get the weight between two nodes
	public int getWeight(int v1, int v2) {
		int weight = matrix[v1][v2];
		return weight==0?0:(weight== MAX_WEIGHT?-1:weight);
		
	}
	
	//get first connected node
	public int getFirstConnectedNode(int index) {
		for(int i=0; i<vertexSize;i++) {
			if(matrix[index][i]>0 && matrix[index][i]<MAX_WEIGHT) {
				return i;
			}
			
		}
		return -1;
	}
	
	/**
	 * get next node
	 * @param v  vertex
	 * @param index node
	 * @return
	 */
	public int getNextNode(int v, int index) {
		for(int i=index+1; i<vertexSize;i++) {
			if(matrix[v][i]>0&&matrix[v][i]<MAX_WEIGHT) {
				return i;
			}
		}
		return -1;
		
	}
	
	// depth first search
	private void depthFirstSearch(int i) {
		isVisited[i]=true;
		int w = getFirstConnectedNode(i);
		while(w!=-1) {
			if(!isVisited[w]) {
				//need traversal
				System.out.println("Found "+w);
				depthFirstSearch(w);
			}
			w= getNextNode(i,w);
		}
	}
	// depth first search for public
	public void depthFirstSearch() {
		isVisited = new boolean[vertexSize];
		for(int i=0;i<vertexSize;i++) {
			if(!isVisited[i]) {
				System.out.println("Found "+i);
				depthFirstSearch(i);
			}
		}
		isVisited = new boolean[vertexSize];
		
	}
	
	//Breadth-First-Search
	public void breadthFirstSearch() {
		isVisited = new boolean[vertexSize];
		for(int i=0;i<vertexSize;i++) {
				if(!isVisited[i]) {
					breadthFirstSearch(i);
				}
		}
		isVisited = new boolean[vertexSize];
	}
    
	//Breadth-First-Search
	private void breadthFirstSearch(int i) {
		int vertex, nextNode;
		LinkedList<Integer> queue = new LinkedList<Integer> ();
		System.out.println("Found "+i);
		isVisited[i] = true;
		queue.add(i);// add v0 to queue;
		while(!queue.isEmpty()) {
			vertex = queue.removeFirst();
			nextNode = getFirstConnectedNode(vertex);
			while(nextNode!=-1) {
				if(!isVisited[nextNode]) {
					System.out.println("Found "+nextNode);
					isVisited[nextNode] = true;
					queue.add(nextNode);
				}
				nextNode = getNextNode(vertex,nextNode);
			}
		}
	}
	
	// Prim's algorithm 
	public void prim() {
		int[] minCost = new int[vertexSize];//minimum cost vertices array
		int[] vertexWeight = new int[vertexSize];//vertices weight array
		int min, minIndex,sum =0;
		for(int i=1;i<vertexSize;i++) {
			minCost[i] = matrix[0][i];
			
		}
		for(int i=1;i<vertexSize;i++) {
			min = MAX_WEIGHT;
			minIndex =0;
			for(int j =1; j<vertexSize;j++) {
				if(minCost[j]<min && minCost[j]>0 ) {
					min = minCost[j];
					minIndex =j;
				}
			}
			System.out.println("Vertex :"+vertexWeight[minIndex]+ " Weight:"+min);
			sum+=min;
			minCost[minIndex]=0;
			for(int j =1; j<vertexSize;j++) {
				if(minCost[j]!=0&&matrix[minIndex][j]<minCost[j] ) {
					minCost[j]=matrix[minIndex][j];
					vertexWeight[j] =minIndex;
				}
			}
		}
		System.out.println("Minimum spanning tree total weight:"+sum);
	}
	public int getVertexSize() {
		return vertexSize;
	}
   
	public void setVertexSize(int vertexSize) {
		this.vertexSize = vertexSize;
	}

	public int[] getVertices() {
		return vertices;
	}

	public void setVertices(int[] vertices) {
		this.vertices = vertices;
	}

	public int[][] getMatrix() {
		return matrix;
	}

	public void setMatrix(int[][] matrix) {
		this.matrix = matrix;
	}

	public int getMAX_WEIGHT() {
		return MAX_WEIGHT;
	}

	public static void main(String[] args) {
		
		Graph graph = new Graph(9);
		int[] a1 = new int[] {0,10,MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT,11,MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT};
		int[] a2 = new int[] {10,0,18,MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT,16,MAX_WEIGHT,12};
		int[] a3 = new int[] {MAX_WEIGHT,MAX_WEIGHT,0,22,MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT,8};
		int[] a4 = new int[] {MAX_WEIGHT,MAX_WEIGHT,22,0,20,MAX_WEIGHT,MAX_WEIGHT,16,21};
		int[] a5 = new int[] {MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT,20,0,26,MAX_WEIGHT,7,MAX_WEIGHT};
		int[] a6 = new int[] {11,MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT,26,0,17,MAX_WEIGHT,MAX_WEIGHT};
		int[] a7 = new int[] {MAX_WEIGHT,16,MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT,17,0,19,MAX_WEIGHT};
		int[] a8 = new int[] {MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT,16,7,MAX_WEIGHT,19,0,MAX_WEIGHT};
		int[] a9 = new int[] {MAX_WEIGHT,12,8,21,MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT,MAX_WEIGHT,0};
		

		graph.matrix[0]  = a1;
		graph.matrix[1]  = a2;
		graph.matrix[2]  = a3;
		graph.matrix[3]  = a4;
		graph.matrix[4]  = a5;
		graph.matrix[5]  = a6;
		graph.matrix[6]  = a7;
		graph.matrix[7]  = a8;
		graph.matrix[8]  = a9;
		
		//System.out.println(graph.getInDegree(1));
		//System.out.println(graph.getWeight(0,4));
		
		//graph.depthFirstSearch();
		//graph.breadthFirstSearch();
		graph.prim();

		

    	}
    }

